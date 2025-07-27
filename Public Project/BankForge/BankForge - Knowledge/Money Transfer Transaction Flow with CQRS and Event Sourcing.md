- [ ] Money Transfer Transaction Flow with CQRS and Event Sourcing ➕ 2025-07-27 📅 2025-07-27 

### Show me example codes of a money transfer transaction including all related modules, using JPA, an event sourcing module, and other relevant technologies.

Based on a banking system architecture, here is a comprehensive example of a money transfer transaction flow in Kotlin using JPA and event sourcing across all related modules:

#### 1\. Shared Common Module - DTOs and Events

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
        // Create transfer aggregate
        val transfer = MoneyTransfer.create(
            id = command.transferId,
            fromAccountId = command.fromAccountId,
            toAccountId = command.toAccountId,
            amount = command.amount,
            currency = command.currency,
            userId = command.userId
        )

        // Save to PostgreSQL
        transferRepository.save(transfer)

        // Publish event to event store and Kafka
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
        // Debit from account
        val fromAccount = accountRepository.findById(event.fromAccountId)
            .orElseThrow { AccountNotFoundException("Account not found: ${event.fromAccountId}") }

        if (fromAccount.balance < event.amount) {
            throw InsufficientFundsException("Insufficient balance")
        }

        fromAccount.debit(event.amount)
        accountRepository.save(fromAccount)

        // Publish debit event
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
        // Credit to destination account
        val transferEvent = getTransferEvent(event.transferId)

        val toAccount = accountRepository.findById(transferEvent.toAccountId)
            .orElseThrow { AccountNotFoundException("Account not found: ${transferEvent.toAccountId}") }

        toAccount.credit(event.amount)
        accountRepository.save(toAccount)

        // Publish credit event
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
        // Save to PostgreSQL for ACID compliance
        val eventEntity = EventEntity(
            id = UUID.randomUUID().toString(),
            aggregateId = aggregateId,
            eventType = eventType,
            eventData = ObjectMapper().writeValueAsString(eventData),
            timestamp = LocalDateTime.now()
        )
        eventRepository.save(eventEntity)

        // Save to MongoDB for flexible querying
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
        // Try Redis cache first
        val cached = redisTemplate.opsForValue().get("transfer:$transferId")
        if (cached != null) {
            return cached as TransferStatusResponse
        }

        // Query from read model
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

        // Cache for 5 minutes
        redisTemplate.opsForValue().set("transfer:$transferId", response, Duration.ofMinutes(5))

        return response
    }
}
```

#### 6\. Notification Service

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
        // Send real-time notification via WebSocket
        webSocketService.sendToUser(event.userId, "Transfer of ${event.amount} initiated")

        // Send email notification
        emailService.sendTransferNotification(event.userId, event)
    }
}
```

#### Key Flow Features

  * **CQRS Pattern**: Separate command and query services.
  * **Event Sourcing**: All state changes are stored as events.
  * **Multi-database Architecture**: PostgreSQL for ACID compliance, MongoDB for events, Redis for caching.
  * **Event-driven Communication**: Kafka for asynchronous processing.
  * **Distributed Transaction Management**: Using the saga pattern with event choreography.
  * **Real-time Notifications**: WebSocket, email, and SMS notifications.

#### Additional Topics for Implementation

  * Saga pattern for transaction rollback scenarios
  * Circuit breakers for external service calls
  * Audit logging and compliance tracking
  * Fraud detection integration
  * Multi-currency support with exchange rates
  * API rate limiting and security
  * Monitoring with Micrometer/Prometheus integration

-----

### Summary

**Architectural Patterns & Concepts**

  * **CQRS (Command Query Responsibility Segregation)**: Write operations (Commands) are separated from read operations (Queries).
  * **Event Sourcing**: System state is determined by a sequence of events, not direct state updates.
  * **Event-Driven Architecture**: Services communicate asynchronously via events.
  * **Saga Pattern (Event Choreography)**: A long-running transaction is managed across services through a sequence of events, where each service reacts to events and publishes new ones.
  * **Multi-Database Architecture**: Utilizes different databases for their specific strengths (PostgreSQL for transactional integrity, MongoDB for flexible event storage, Redis for caching).

**Core Service Modules**

  * **Shared Common Module**: Contains shared data structures like Data Transfer Objects (DTOs) and Event classes to ensure consistency across services.
  * **Transaction Command Service**:
      * Initiates the transfer process via a REST endpoint.
      * Creates and persists the initial `MoneyTransfer` aggregate.
      * Publishes the `MoneyTransferRequestedEvent` to start the saga.
  * **Account Command Service**:
      * Listens for transfer-related events from Kafka.
      * Handles the core logic of debiting the source account and crediting the destination account.
      * Publishes `AccountDebitedEvent` and `AccountCreditedEvent` to signal progress.
  * **Event Store Service**:
      * Listens for all system events.
      * Persists every event to both PostgreSQL (for ACID-compliant logging) and MongoDB (for easy querying and analysis).
  * **Transaction Query Service**:
      * Provides read-only endpoints to check transfer status and history.
      * Reads from a denormalized read model (not shown, but implied).
      * Uses Redis for caching to improve read performance.
  * **Notification Service**:
      * Listens for key business events (`MoneyTransferRequestedEvent`, `AccountDebitedEvent`, etc.).
      * Sends real-time user notifications via various channels like WebSockets, email, or SMS.

**Technology Stack**

  * **Language**: Kotlin
  * **Primary Database (ACID)**: PostgreSQL (managed with JPA)
  * **Event Query Database**: MongoDB
  * **Caching Layer**: Redis
  * **Messaging Bus**: Apache Kafka