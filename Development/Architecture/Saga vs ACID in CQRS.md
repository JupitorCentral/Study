---
tags:
  - architecture
  - cqrs
  - saga
  - acid
  - patterns
---

### CQRS 아키텍처에서 @Service 레이어는 이벤트 트리거 외에 어떤 역할을 하나요?

Event Sourcing이 적용된 CQRS 아키텍처에서 `@Service` 레이어는 단순히 이벤트를 발행하는 것 이상의 중요한 역할을 수행합니다. 주된 책임은 **비즈니스 로직 처리**와 트랜잭션 핸들러로 넘어가기 전의 **명령(Command) 조율**입니다.

#### 주요 책임 (Primary Responsibilities)

- **비즈니스 로직 검증 (Validation):** 들어오는 요청이 비즈니스 규칙을 준수하는지 검증하여 데이터 무결성을 보장합니다. 여러 필드나 엔티티에 걸친 복잡한 검증 로직이 포함될 수 있습니다.
    
- **명령 조율 (Command Orchestration):** 요청 DTO(Data Transfer Object)를 도메인 전용 명령(Command) 객체로 변환합니다. 필요하다면 여러 작업을 조율하거나 서로 다른 도메인 애그리거트 간의 흐름을 관리합니다.
    
- **횡단 관심사 (Cross-Cutting Concerns):** 권한 확인(Authorization), 로깅, 입력 값 정제(Sanitization), 에러 핸들링 등을 처리하기에 가장 적합한 위치입니다.
    

#### Kotlin 구현 패턴

다음은 `AccountService`가 요청을 검증하고, 명령을 생성하여 핸들러에게 전달하는 예시입니다.

```kotlin
@Service
class AccountService(
    private val commandHandler: AccountCommandHandler
) {
    fun createAccount(request: CreateAccountRequest): AccountResponse {
        // 1. 비즈니스 규칙 검증
        validateAccountCreation(request)

        // 2. 도메인 명령(Command)으로 변환
        val command = CreateAccountCommand(
            accountId = generateAccountId(),
            customerId = request.customerId,
            initialBalance = request.initialBalance
        )

        // 3. 명령 실행 (이때 이벤트가 트리거됨)
        val result = commandHandler.handle(command)

        // 4. 결과를 응답으로 변환
        return AccountResponse.from(result)
    }
}
```

#### 핵심 차이점

서비스 레이어는 **비즈니스 로직**에 집중하는 반면, 트랜잭션 핸들러(이벤트 소싱 모듈)는 **이벤트의 영속화 및 발행(예: Kafka)**에 집중합니다. 서비스 레이어는 이벤트 버스와 직접 상호작용하지 않는 것이 일반적입니다.

---

### Saga 패턴과 Cross-Aggregate 트랜잭션이란 무엇인가요?

#### Saga 패턴

Saga 패턴은 분산 환경에서 마이크로서비스 간의 데이터 일관성을 관리하기 위한 디자인 패턴입니다. 분산 환경에서는 전통적인 ACID 트랜잭션을 적용하기 어렵기 때문에 그 대안으로 사용됩니다.

- **핵심 개념:** 거대한 트랜잭션을 여러 개의 작은 **로컬 트랜잭션**으로 쪼갭니다. 각 로컬 트랜잭션은 실패 시 이전 단계들을 되돌리기 위한 **보상 트랜잭션(Compensating Action)**을 가지고 있습니다.
    
- **Kotlin 예시:** 계좌 이체 Saga는 A 계좌 출금 후 B 계좌 입금을 수행합니다. 만약 입금이 실패하면, 출금을 취소(보상)해 주어야 합니다.
    
    ```kotlin
    // 예: 계좌 이체 Saga
    class MoneyTransferSaga {
        fun execute(transfer: TransferRequest) {
            // 1단계: 출금 계좌에서 차감
            // 2단계: 입금 계좌에 추가
            // 만약 2단계 실패 시 -> 1단계 보상 로직(환불) 실행
        }
    }
    ```
    

#### Cross-Aggregate 트랜잭션

단일 비즈니스 작업이 여러 개의 독립적인 **도메인 애그리거트(Aggregate)**를 수정해야 하는 경우를 말합니다. 각 애그리거트는 자신만의 일관성 경계를 가지므로, 단일 원자적(Atomic) 트랜잭션으로 묶기 어렵습니다. 이때 Saga와 같은 조율 패턴이 필요합니다.

- **뱅킹 예시:** 하나의 비즈니스 프로세스에서 다음 작업들이 모두 필요할 때:
    1. 계좌 간 송금 (**Account** Aggregate)
    2. 고객 신용 점수 업데이트 (**Customer** Aggregate)
    3. 거래 내역 기록 (**Transaction** Aggregate)
        
- **구현 방식:**
    - **Choreography (안무):** 중앙 관리자 없이 서비스끼리 이벤트를 주고받으며 진행.
    - **Orchestration (지휘):** 중앙 오케스트레이터 서비스가 전체 흐름을 제어.
        

---

### Saga 패턴과 ACID 패턴의 정의 및 비교

#### ACID 패턴

데이터베이스 트랜잭션이 안정적으로 처리됨을 보장하는 4가지 성질입니다.

- **Atomicity (원자성):** 모든 작업이 성공하거나, 아니면 전부 실패해야 합니다 (All-or-Nothing).
- **Consistency (일관성):** 트랜잭션 전후로 데이터베이스의 무결성이 유지되어야 합니다.
- **Isolation (격리성):** 동시에 실행되는 트랜잭션끼리는 서로 영향을 주지 않아야 합니다.
- **Durability (지속성):** 커밋된 트랜잭션은 영구적으로 저장되어야 합니다.

- **ACID 예시 (Kotlin):** 단일 서비스 내에서의 이체. `@Transactional` 하나로 모든 것이 보장됩니다.
    ```kotlin
    @Transactional
    fun transferMoney(...) {
        debit(...)  // 1
        credit(...) // 2
        // 실패 시 자동 롤백
    }
    ```

#### Saga 패턴

분산된 트랜잭션을 로컬 트랜잭션의 연속으로 관리합니다. 각 단계는 이벤트를 통해 다음 단계를 트리거하며, 실패 시 보상 트랜잭션으로 되돌립니다.

- **Saga 예시:** 주문 프로세스 (재고 -> 결제 -> 배송).
    ```kotlin
    class OrderSaga {
        fun processOrder(order: Order) {
            reserveInventory(order) // 1
            processPayment(order)   // 2
            shipOrder(order)        // 3
            // 3번 실패 시 -> 결제 취소, 재고 복구 실행
        }
    }
    ```

#### 핵심 비교

|구분|ACID|Saga|
|---|---|---|
|**범위**|단일 데이터베이스 / 모놀리식 서비스|여러 서비스 / 분산 시스템|
|**일관성**|즉시 일관성 (Immediate) / 강력함|결과적 일관성 (Eventual Consistency)|
|**격리성**|완벽한 격리 (중간 상태 노출 X)|격리 없음 (중간 단계가 커밋되어 보일 수 있음)|
|**롤백**|자동 (DB가 처리)|수동 (보상 로직을 직접 구현해야 함)|
|**복잡도**|단순함 (어노테이션 하나로 해결)|복잡함 (상태 관리 및 보상 로직 필요)|
|**사용처**|단일 서비스 내의 작업|마이크로서비스, 장기 실행 프로세스|

---

### Kafka와 마이크로서비스로 Saga를 구현할 때, '배송' 단계가 실패하면 이전 단계를 취소하는 보상 이벤트가 필요한가요?

네, 정확합니다. Kafka를 사용하는 마이크로서비스 아키텍처에서 Saga의 특정 단계가 실패하면, 이전 단계들을 되돌리기 위해 **보상 이벤트(Compensation Events)**를 발행해야 합니다.

#### 이벤트 기반 보상 처리

만약 `shipOrder` 단계가 실패하면, Saga 오케스트레이터(혹은 해당 서비스)가 롤백을 시작하는 이벤트를 발행합니다.

- **오케스트레이터 로직 (Kotlin):**
    ```kotlin
    class OrderSaga {
        fun handleShipOrderFailed(orderId: String) {
            // 보상 이벤트 발행
            eventPublisher.publish(CancelPaymentEvent(orderId))
            eventPublisher.publish(ReleaseInventoryEvent(orderId))
        }
    }
    ```

#### Kafka 이벤트 흐름

- **성공 경로:**
    `OrderCreated` → `InventoryReserved` → `PaymentProcessed` → `OrderShipped`
    
- **실패 경로 (배송 실패 시):**
    `ShipOrderFailed` → `CancelPaymentEvent` → `ReleaseInventoryEvent`
    

#### 이벤트 핸들러

각 서비스는 자신의 보상 이벤트를 리슨하고 있다가, 역방향 작업(환불 등)을 수행합니다.

---

### 만약 보상 트랜잭션(환불 등) 자체가 실패하면 어떡하나요?

보상 작업(예: 결제 환불)마저 실패하는 상황에 대비해 강력한 실패 처리 전략이 필수적입니다.

#### Dead-Letter Queue (DLQ)

처리되지 못한 보상 이벤트(`CancelPaymentEvent`)는 **DLQ (죽은 편지함)**라는 별도의 Kafka 토픽으로 이동시킵니다. 별도의 컨슈머가 이를 모니터링합니다.

#### 지수 백오프를 이용한 재시도 (Retry with Back-off)

일시적인 오류일 수 있으므로, 시스템은 점진적으로 대기 시간을 늘려가며(Exponential Back-off) 재시도를 수행합니다. 이때 핸들러는 반드시 **멱등성(Idempotent)**을 가져야 합니다. (예: 환불을 두 번 요청해도 한 번만 처리되도록)

#### 알림 및 수동 개입 (Alerting)

자동 재시도도 모두 실패하면, 시스템은 개발자나 운영팀에게 **경고(Alert)**를 보냅니다. "주문 ID 123의 환불이 실패했습니다"라는 메시지를 받고 엔지니어가 수동으로 개입해야 합니다.

#### 타임아웃 및 폴백 (Fallback)

보상 프로세스에 시간 제한을 두고, 초과 시 '실패' 상태로 영구 기록하거나 대체 비즈니스 프로세스를 트리거합니다.

---

### 요약 (Summary)

**CQRS 서비스 레이어의 역할**

- **비즈니스 로직 검증:** 규칙 및 제약 조건 확인.
- **명령 조율:** 요청을 도메인 명령으로 변환 및 조율.
- **횡단 관심사:** 로깅, 권한 등 처리.
- **분리:** 비즈니스 로직과 이벤트 발행/저장을 분리.

**Saga 패턴**

- **목적:** 분산 시스템에서 데이터 일관성 관리.
- **메커니즘:** 로컬 트랜잭션의 연속. 실패 시 **보상 트랜잭션**으로 되돌림.
- **특징:** **결과적 일관성**을 가지며, 롤백을 수동으로 구현해야 함.

**ACID 패턴**

- **특징:** 원자성, 일관성, 격리성, 지속성.
- **사용:** 단일 DB 트랜잭션 내에서 완벽한 일관성 보장.

**마이크로서비스에서의 실패 처리**

- **보상 이벤트:** 실패 시 이전 단계를 취소하는 이벤트 발행.
- **재시도:** 실패한 보상 작업은 자동 재시도 (멱등성 필수).
- **DLQ:** 재시도 실패 시 별도 큐로 격리.
- **알림:** 최후의 수단으로 운영자에게 알림 발송.