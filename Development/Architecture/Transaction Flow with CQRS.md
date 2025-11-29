---
tags:
  - architecture
  - cqrs
  - event-sourcing
  - transaction
---

### JPA, Event Sourcing 모듈 등을 사용한 송금 트랜잭션의 전체 흐름 예시 코드

뱅킹 시스템 아키텍처를 기반으로, Kotlin, JPA, Event Sourcing 및 관련 기술을 모두 활용한 포괄적인 송금(Money Transfer) 트랜잭션 흐름 예시입니다.

#### 1\. Shared Common Module - DTOs and Events
(모든 서비스가 공유하는 데이터 구조 및 이벤트 정의)

```kotlin
// shared-common/src/main/kotlin/com/bankforge/common/events/TransferEvents.kt
data class MoneyTransferRequestedEvent(
    val transferId: String,
    val fromAccountId: String,
    val toAccountId: String,
    val amount: BigDecimal,
    val currency: String,
    val timestamp: LocalDateTime,
    val userId: String
)

data class AccountDebitedEvent(
    val accountId: String,
    val transferId: String,
    val amount: BigDecimal,
    val newBalance: BigDecimal,
    val timestamp: LocalDateTime
)

data class AccountCreditedEvent(
    val accountId: String,
    val transferId: String,
    val amount: BigDecimal,
    val newBalance: BigDecimal,
    val timestamp: LocalDateTime
)

data class MoneyTransferCompletedEvent(
    val transferId: String,
    val status: TransferStatus,
    val timestamp: LocalDateTime
)

// shared-common/src/main/kotlin/com/bankforge/common/dto/TransferRequest.kt
data class TransferRequest(
    val fromAccountId: String,
    val toAccountId: String,
    val amount: BigDecimal,
    val currency: String = "USD"
)

enum class TransferStatus {
    REQUESTED, DEBITED, CREDITED, COMPLETED, FAILED
}
```

#### 2\. Transaction Command Service
(트랜잭션 요청을 받고 초기 상태를 저장하며 이벤트를 발행하는 서비스)

```kotlin
// transaction-command-service/src/main/kotlin/com/bankforge/transaction/command/controller/TransactionController.kt
@RestController
@RequestMapping("/api/v1/transactions")
class TransactionController(
    private val transferCommandHandler: MoneyTransferCommandHandler
) {
    @PostMapping("/transfer")
    fun initiateTransfer(
        @RequestBody request: TransferRequest,
        @RequestHeader("Authorization") token: String
    ): ResponseEntity<String> {
        val transferId = UUID.randomUUID().toString()
        val userId = extractUserIdFromToken(token)

        val command = InitiateTransferCommand(
            transferId = transferId,
            fromAccountId = request.fromAccountId,
            toAccountId = request.toAccountId,
            amount = request.amount,
            currency = request.currency,
            userId = userId
        )

        transferCommandHandler.handle(command)
        return ResponseEntity.ok(transferId)
    }
}

// transaction-command-service/src/main/kotlin/com/bankforge/transaction/command/handler/MoneyTransferCommandHandler.kt
@Service
@Transactional
class MoneyTransferCommandHandler(
    private val transferRepository: MoneyTransferRepository,
    private val eventPublisher: ApplicationEventPublisher,
    private val kafkaTemplate: KafkaTemplate<String, Any>
) {
    fun handle(command: InitiateTransferCommand) {
        // 이체 Aggregate 생성
        val transfer = MoneyTransfer.create(
            id = command.transferId,
            fromAccountId = command.fromAccountId,
            toAccountId = command.toAccountId,
            amount = command.amount,
            currency = command.currency,
            userId = command.userId
        )

        // PostgreSQL에 저장 (ACID 보장)
        transferRepository.save(transfer)

        // 이벤트 저장소 및 Kafka로 이벤트 발행
        val event = MoneyTransferRequestedEvent(
            transferId = command.transferId,
            fromAccountId = command.fromAccountId,
            toAccountId = command.toAccountId,
            amount = command.amount,
            currency = command.currency,
            timestamp = LocalDateTime.now(),
            userId = command.userId
        )

        eventPublisher.publishEvent(event)
        kafkaTemplate.send("transfer-events", event)
    }
}

// transaction-command-service/src/main/kotlin/com/bankforge/transaction/command/aggregate/MoneyTransfer.kt
@Entity
@Table(name = "money_transfers")
data class MoneyTransfer(
    @Id
    val id: String,
    val fromAccountId: String,
    val toAccountId: String,
    val amount: BigDecimal,
    val currency: String,
    @Enumerated(EnumType.STRING)
    var status: TransferStatus,
    val userId: String,
    val createdAt: LocalDateTime,
    var updatedAt: LocalDateTime
) {
    companion object {
        fun create(
            id: String,
            fromAccountId: String,
            toAccountId: String,
            amount: BigDecimal,
            currency: String,
            userId: String
        ): MoneyTransfer {
            return MoneyTransfer(
                id = id,
                fromAccountId = fromAccountId,
                toAccountId = toAccountId,
                amount = amount,
                currency = currency,
                status = TransferStatus.REQUESTED,
                userId = userId,
                createdAt = LocalDateTime.now(),
                updatedAt = LocalDateTime.now()
            )
        }
    }

    fun markAsDebited() {
        this.status = TransferStatus.DEBITED
        this.updatedAt = LocalDateTime.now()
    }

    fun markAsCompleted() {
        this.status = TransferStatus.COMPLETED
        this.updatedAt = LocalDateTime.now()
    }
}
```

#### 3\. Account Command Service
(Kafka 이벤트를 수신하여 실제 계좌의 입출금을 처리하는 서비스)

```kotlin
// account-command-service/src/main/kotlin/com/bankforge/account/command/handler/AccountEventHandler.kt
@Component
class AccountEventHandler(
    private val accountRepository: AccountRepository,
    private val eventPublisher: ApplicationEventPublisher,
    private val kafkaTemplate: KafkaTemplate<String, Any>
) {
    @KafkaListener(topics = ["transfer-events"])
    @Transactional
    fun handleTransferRequested(event: MoneyTransferRequestedEvent) {
        // 출금 계좌에서 차감 (Debit)
        val fromAccount = accountRepository.findById(event.fromAccountId)
            .orElseThrow { AccountNotFoundException("Account not found: ${event.fromAccountId}") }

        if (fromAccount.balance < event.amount) {
            throw InsufficientFundsException("Insufficient balance")
        }

        fromAccount.debit(event.amount)
        accountRepository.save(fromAccount)

        // 차감 이벤트 발행
        val debitEvent = AccountDebitedEvent(
            accountId = event.fromAccountId,
            transferId = event.transferId,
            amount = event.amount,
            newBalance = fromAccount.balance,
            timestamp = LocalDateTime.now()
        )

        eventPublisher.publishEvent(debitEvent)
        kafkaTemplate.send("account-events", debitEvent)
    }

    @KafkaListener(topics = ["account-events"])
    @Transactional
    fun handleAccountDebited(event: AccountDebitedEvent) {
        // 입금 계좌에 입금 (Credit)
        // 실제로는 transferId로 대상 계좌 정보를 찾아야 함 (여기서는 단순화됨)
        val transferEvent = getTransferEvent(event.transferId)

        val toAccount = accountRepository.findById(transferEvent.toAccountId)
            .orElseThrow { AccountNotFoundException("Account not found: ${transferEvent.toAccountId}") }

        toAccount.credit(event.amount)
        accountRepository.save(toAccount)

        // 입금 이벤트 발행
        val creditEvent = AccountCreditedEvent(
            accountId = transferEvent.toAccountId,
            transferId = event.transferId,
            amount = event.amount,
            newBalance = toAccount.balance,
            timestamp = LocalDateTime.now()
        )

        eventPublisher.publishEvent(creditEvent)
        kafkaTemplate.send("account-events", creditEvent)
    }
}

// account-command-service/src/main/kotlin/com/bankforge/account/command/aggregate/Account.kt
@Entity
@Table(name = "accounts")
data class Account(
    @Id
    val id: String,
    val userId: String,
    var balance: BigDecimal,
    val currency: String,
    var updatedAt: LocalDateTime
) {
    fun debit(amount: BigDecimal) {
        if (balance < amount) {
            throw InsufficientFundsException("Insufficient balance")
        }
        balance = balance.subtract(amount)
        updatedAt = LocalDateTime.now()
    }

    fun credit(amount: BigDecimal) {
        balance = balance.add(amount)
        updatedAt = LocalDateTime.now()
    }
}
```

#### 4\. Event Store Service
(모든 이벤트를 영구 저장하는 서비스)

```kotlin
// event-store-service/src/main/kotlin/com/bankforge/eventstore/service/EventStoreService.kt
@Service
@Transactional
class EventStoreService(
    private val eventRepository: EventRepository,
    private val mongoTemplate: MongoTemplate
) {
    @EventListener
    fun handleTransferEvent(event: MoneyTransferRequestedEvent) {
        saveEvent("MoneyTransferRequested", event.transferId, event)
    }

    @EventListener
    fun handleAccountDebitedEvent(event: AccountDebitedEvent) {
        saveEvent("AccountDebited", event.transferId, event)
    }

    @EventListener
    fun handleAccountCreditedEvent(event: AccountCreditedEvent) {
        saveEvent("AccountCredited", event.transferId, event)
    }

    private fun saveEvent(eventType: String, aggregateId: String, eventData: Any) {
        // PostgreSQL에 저장 (ACID 준수, 구조적 데이터)
        val eventEntity = EventEntity(
            id = UUID.randomUUID().toString(),
            aggregateId = aggregateId,
            eventType = eventType,
            eventData = ObjectMapper().writeValueAsString(eventData),
            timestamp = LocalDateTime.now()
        )
        eventRepository.save(eventEntity)

        // MongoDB에 저장 (유연한 쿼리, 문서형 데이터)
        val mongoEvent = MongoEvent(
            id = eventEntity.id,
            aggregateId = aggregateId,
            eventType = eventType,
            eventData = eventData,
            timestamp = LocalDateTime.now()
        )
        mongoTemplate.save(mongoEvent, "events")
    }
}

// event-store-service/src/main/kotlin/com/bankforge/eventstore/entity/EventEntity.kt
@Entity
@Table(name = "events")
data class EventEntity(
    @Id
    val id: String,
    val aggregateId: String,
    val eventType: String,
    @Column(columnDefinition = "TEXT")
    val eventData: String,
    val timestamp: LocalDateTime
)
```

#### 5\. Transaction Query Service
(읽기 전용 서비스, Redis 캐싱 사용)

```kotlin
// transaction-query-service/src/main/kotlin/com/bankforge/transaction/query/controller/TransactionQueryController.kt
@RestController
@RequestMapping("/api/v1/transactions")
class TransactionQueryController(
    private val transferQueryService: TransferQueryService
) {
    @GetMapping("/{transferId}")
    fun getTransferStatus(@PathVariable transferId: String): ResponseEntity<TransferStatusResponse> {
        val status = transferQueryService.getTransferStatus(transferId)
        return ResponseEntity.ok(status)
    }

    @GetMapping("/user/{userId}")
    fun getUserTransfers(@PathVariable userId: String): ResponseEntity<List<TransferHistoryResponse>> {
        val transfers = transferQueryService.getUserTransfers(userId)
        return ResponseEntity.ok(transfers)
    }
}

// transaction-query-service/src/main/kotlin/com/bankforge/transaction/query/service/TransferQueryService.kt
@Service
class TransferQueryService(
    private val transferReadRepository: TransferReadModelRepository,
    private val redisTemplate: RedisTemplate<String, Any>
) {
    fun getTransferStatus(transferId: String): TransferStatusResponse {
        // 1. Redis 캐시 먼저 확인
        val cached = redisTemplate.opsForValue().get("transfer:$transferId")
        if (cached != null) {
            return cached as TransferStatusResponse
        }

        // 2. Read Model DB(예: MongoDB/PostgreSQL View) 조회
        val transfer = transferReadRepository.findById(transferId)
            .orElseThrow { TransferNotFoundException("Transfer not found: $transferId") }

        val response = TransferStatusResponse(
            transferId = transfer.id,
            status = transfer.status,
            amount = transfer.amount,
            fromAccountId = transfer.fromAccountId,
            toAccountId = transfer.toAccountId,
            timestamp = transfer.createdAt
        )

        // 3. 캐시에 저장 (5분간 유효)
        redisTemplate.opsForValue().set("transfer:$transferId", response, Duration.ofMinutes(5))

        return response
    }
}
```

#### 6\. Notification Service
(사용자 알림 처리)

```kotlin
// notification-service/src/main/kotlin/com/bankforge/notification/handler/TransferNotificationHandler.kt
@Component
class TransferNotificationHandler(
    private val emailService: EmailService,
    private val smsService: SmsService,
    private val webSocketService: WebSocketService
) {
    @KafkaListener(topics = ["transfer-events", "account-events"])
    fun handleTransferEvents(event: Any) {
        when (event) {
            is MoneyTransferRequestedEvent -> {
                sendTransferInitiatedNotification(event)
            }
            is MoneyTransferCompletedEvent -> {
                sendTransferCompletedNotification(event)
            }
            is AccountDebitedEvent -> {
                sendBalanceUpdateNotification(event)
            }
        }
    }

    private fun sendTransferInitiatedNotification(event: MoneyTransferRequestedEvent) {
        // WebSocket으로 실시간 알림 전송
        webSocketService.sendToUser(event.userId, "Transfer of ${event.amount} initiated")

        // 이메일 알림 전송
        emailService.sendTransferNotification(event.userId, event)
    }
}
```

#### 주요 흐름 특징 (Key Flow Features)

  * **CQRS 패턴:** 명령(Command) 서비스와 조회(Query) 서비스가 분리되어 있음.
  * **Event Sourcing:** 모든 상태 변경이 이벤트로 저장됨.
  * **Multi-database Architecture:** 
      * PostgreSQL: 트랜잭션 무결성(ACID) 보장.
      * MongoDB: 유연한 이벤트 저장.
      * Redis: 조회 성능을 위한 캐싱.
  * **Event-driven Communication:** Kafka를 통한 비동기 처리.
  * **분산 트랜잭션 관리:** 이벤트 안무(Choreography)를 이용한 Saga 패턴 적용.
  * **실시간 알림:** WebSocket, 이메일, SMS를 통한 알림.

#### 추가 구현 고려사항

  * 트랜잭션 롤백 시나리오를 위한 Saga 패턴 보상 트랜잭션 로직.
  * 외부 서비스 호출을 위한 서킷 브레이커(Circuit Breakers).
  * 감사 로그(Audit logging) 및 규정 준수 추적.
  * 이상 거래 탐지(Fraud detection) 통합.
  * 환율을 고려한 다중 통화 지원.
  * API 속도 제한 및 보안.
  * Micrometer/Prometheus를 이용한 모니터링 통합.

-----

### 요약 (Summary)

**아키텍처 패턴 및 개념**

  * **CQRS (Command Query Responsibility Segregation):** 쓰기 작업(명령)과 읽기 작업(조회)을 분리합니다.
  * **Event Sourcing:** 시스템의 상태를 직접 업데이트하는 것이 아니라, 일련의 이벤트 순서로 결정합니다.
  * **Event-Driven Architecture:** 서비스들이 이벤트를 통해 비동기적으로 통신합니다.
  * **Saga Pattern (Event Choreography):** 서비스 간에 이벤트를 주고받으며 장기 실행 트랜잭션을 관리합니다.
  * **Multi-Database Architecture:** 각 DB의 장점을 활용 (PostgreSQL-트랜잭션, MongoDB-이벤트, Redis-캐시).

**핵심 서비스 모듈**

  * **Shared Common Module:** 모든 서비스에서 일관성을 유지하기 위해 DTO와 Event 클래스를 공유합니다.
  * **Transaction Command Service:**
      * REST 엔드포인트를 통해 이체 프로세스를 시작합니다.
      * 초기 `MoneyTransfer` Aggregate를 생성하고 저장합니다.
      * `MoneyTransferRequestedEvent`를 발행하여 Saga를 시작합니다.
  * **Account Command Service:**
      * Kafka에서 이체 관련 이벤트를 리슨합니다.
      * 출금 및 입금의 핵심 로직을 처리합니다.
      * `AccountDebitedEvent`, `AccountCreditedEvent`를 발행하여 진행 상황을 알립니다.
  * **Event Store Service:**
      * 모든 시스템 이벤트를 리슨합니다.
      * ACID 준수 로깅을 위해 PostgreSQL에, 쉬운 조회 및 분석을 위해 MongoDB에 이벤트를 저장합니다.
  * **Transaction Query Service:**
      * 이체 상태 및 내역 조회를 위한 읽기 전용 엔드포인트를 제공합니다.
      * 역정규화된 읽기 모델(Read model)을 조회합니다.
      * 성능 향상을 위해 Redis 캐싱을 사용합니다.
  * **Notification Service:**
      * 주요 비즈니스 이벤트를 리슨하고 사용자에게 실시간 알림(WebSocket, Email 등)을 전송합니다.

**기술 스택**

  * **언어:** Kotlin
  * **Primary Database (ACID):** PostgreSQL (JPA 사용)
  * **Event Query Database:** MongoDB
  * **Caching Layer:** Redis
  * **Messaging Bus:** Apache Kafka