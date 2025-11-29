---
tags:
  - architecture
  - kafka
  - spring
  - event-sourcing
--- 

### Event Sourcing 패턴이란 무엇이며, 이 프로젝트의 event-store-service와 어떤 관계가 있는가?

**Event Sourcing**은 데이터의 '현재 상태'만을 저장하는 대신, 시스템에 발생한 모든 변경 사항(이벤트)을 순서대로 기록하여 저장하는 아키텍처 패턴입니다. 이를 통해 강력한 감사 추적(Audit trail)과 상태 재구성(State reconstruction)이 가능해집니다.

#### Event Sourcing의 핵심 특징:

- **불변의 이벤트 로그 (Immutable event log):** 모든 변경 사항은 수정 불가능한 이벤트로 기록됩니다.
    
- **상태 재구성 (State reconstruction):** 처음부터 이벤트를 다시 재생(replay)하여 현재 상태를 완벽하게 복원할 수 있습니다.
    
- **완벽한 감사 추적 (Complete audit trail):** 시스템의 모든 작업이 보존되어 규정 준수 및 디버깅에 유리합니다.
    
- **타임 트래블 (Time-travel capabilities):** 특정 시점의 시스템 상태를 조회할 수 있습니다.
    

#### 뱅킹 프로젝트와 event-store-service의 관계

이 프로젝트에서 `event-store-service` 모듈은 Event Sourcing을 구현하는 핵심 컴포넌트입니다. 모든 뱅킹 이벤트의 중앙 저장소 역할을 하며, 시스템의 전체 변경 이력을 관리합니다.

#### 핵심 책임 (Core Responsibilities)

- **이벤트 저장:** 뱅킹 시스템 전반에서 발생하는 모든 트랜잭션 및 도메인 이벤트를 캡처하고 저장합니다.
    
- **이벤트 리플레이:** 저장된 이벤트를 재생하여 시스템 상태를 재구성합니다.
    
- **감사 추적:** 규정 준수를 위해 모든 뱅킹 작업에 대한 불변의 기록을 제공합니다.
    
- **상태 복구:** 전체 이벤트 이력을 유지함으로써 시스템 복구 및 디버깅을 지원합니다.
    

#### CQRS 아키텍처와의 통합

`event-store-service`는 **CQRS (명령과 조회의 책임 분리)** 패턴 안에서 다음과 같이 작동합니다:

- **Command Side (명령):** 쓰기 작업(계좌 생성, 이체 등)은 이벤트를 생성하여 이벤트 저장소에 저장합니다.
    
- **Query Side (조회):** 읽기 작업은 이벤트 저장소의 이벤트를 기반으로 구축된 프로젝션(Projections)을 통해 최적화됩니다.
    
- **이벤트 기반 통신:** 저장소의 이벤트는 Kafka를 통해 다른 모듈로 전파되어 업데이트를 트리거합니다.
    

#### 기술적 구현 (Technical Implementation)

- **MongoDB:** 다양한 이벤트 구조를 유연하게 저장하기 위해 스키마 유연성이 높은 MongoDB를 사용합니다.
    
- **Kafka 통합:** 실시간 이벤트 스트리밍 및 메시지 큐 처리를 위해 Kafka를 사용합니다.
    
- **Spring Boot:** Kotlin 기반의 서비스 구현을 위해 Spring Boot 프레임워크를 사용합니다.
    

#### 관련 학습 주제

- Event Store 데이터베이스 설계 패턴
    
- 이벤트 버저닝 및 스키마 진화 (Schema evolution)
    
- 성능 최적화를 위한 스냅샷(Snapshotting)
    
- 이벤트 리플레이 전략 및 성능 고려사항
    
- 마이크로서비스 간 통합 패턴
    

### 그렇다면, event-store-service가 모든 트랜잭션을 캡처해서 DB에 저장한다는 뜻인가요?

네, 정확합니다! `event-store-service`는 모든 트랜잭션과 도메인 이벤트를 캡처하여 데이터베이스에 저장합니다. 하지만 단순히 "트랜잭션 정보"를 저장하는 것보다 훨씬 정교한 방식입니다.

#### 캡처되는 내용 (What Gets Captured)

- **뱅킹 트랜잭션:** 입금, 출금, 이체 등.
    
- **계좌 수명주기 이벤트:** 계좌 생성, 해지, 상태 변경.
    
- **인증 이벤트:** 로그인 시도, 비밀번호 변경.
    
- **관리자 작업:** 사용자 관리, 설정 변경.
    
- **시스템 이벤트:** 에러 상황, 서비스 간 상호작용.
    

#### 작동 방식 (How It Works)

단순한 데이터값(예: 잔액)을 저장하는 것이 아니라, **무슨 일이 일어났는지**를 나타내는 **불변의 이벤트**를 저장합니다:

```plaintext
저장 방식 비교:
기존 방식: "계좌 잔액 = $500" (현재 상태만 덮어씀)
이벤트 소싱: "MoneyDepositedEvent: $100가 Account123에 입금됨, 시간: 2025-07-22T19:08:00" (이력 추가)
```

#### 주요 이점

- **완벽한 감사 추적:** 모든 행동이 영구적으로 기록됩니다.
    
- **상태 재구성:** 언제든 이벤트를 다시 재생하여 계좌의 현재 상태를 복구할 수 있습니다.
    
- **규정 준수:** 은행은 감사를 위해 상세한 거래 내역이 필요합니다.
    
- **디버깅:** 문제가 발생했을 때 정확히 무슨 일이 있었는지 추적할 수 있습니다.
    
- **타임 트래블:** 과거 특정 시점의 계좌 상태를 확인할 수 있습니다.
    

#### 데이터베이스 저장 구조

MongoDB 구현체에서는 각 이벤트가 다음과 같은 문서(Document)로 저장됩니다:

- **Event Type:** "AccountCreated", "MoneyTransferred" 등.
    
- **Event Data:** 금액, 계좌 ID, 타임스탬프 등.
    
- **Metadata:** 이벤트 ID, 상관관계 ID(Correlation ID), 버전.
    
- **Timestamp:** 이벤트 발생 시간.
    

이 방식은 단순히 현재 잔액만 저장하는 것보다 훨씬 강력한, 변경 불가능한 기록을 제공합니다.

### 실제 구현은 어떻게 하나요? 코드는 보여주지 말고 방법론을 설명해주세요. Spring Bean을 정의해서 캡처 모듈을 구현하나요?

네, **Spring Bean은 뱅킹 시스템에서 이벤트 캡처 모듈을 구현하는 주된 메커니즘**입니다. 아키텍처의 작동 방식은 다음과 같습니다:

#### 핵심 구현 전략 (Core Implementation Strategy)

1. **Spring Bean으로서의 Event Producer**
    
    뱅킹 서비스(계좌 서비스, 트랜잭션 서비스)는 비즈니스 작업 시 자동으로 이벤트를 캡처하고 발행하는 Spring Bean으로 구현됩니다. 각 서비스 Bean은:
    
    - 비즈니스 로직(계좌 생성, 트랜잭션 처리)을 수행합니다.
        
    - 상태 변경에 대한 이벤트를 자동으로 생성합니다.
        
    - 주입된(injected) Kafka Producer를 사용해 이벤트를 발행합니다.
        
    - DB 쓰기와 이벤트 발행 간의 트랜잭션 일관성을 유지합니다.
        
2. **Kafka Template 통합**
    
    Spring은 `KafkaTemplate` Bean을 제공하여 자동 설정 및 주입을 지원합니다. 이 템플릿은:
    
    - 직렬화 및 메시지 포맷팅을 처리합니다.
        
    - Kafka 브로커와의 커넥션 풀을 관리합니다.
        
    - **트랜잭션 기반 발행(Transactional publishing)**을 지원하여, DB 작업이 성공했을 때만 이벤트를 전송하도록 보장합니다.
        
    - 중복 이벤트를 방지하기 위해 **멱등성 프로듀서(Idempotent producers)**를 지원합니다.
        
3. **처리를 위한 Event Listener**
    
    `event-store-service`는 `@KafkaListener` 어노테이션이 붙은 Bean을 사용하여 토픽에서 이벤트를 소비(consume)합니다. 이 리스너 Bean들은:
    
    - 들어오는 이벤트를 자동으로 역직렬화합니다.
        
    - 적절한 메타데이터와 함께 이벤트를 MongoDB에 저장합니다.
        
    - 안정적인 처리를 위해 **ACK 패턴(Acknowledgment patterns)**을 처리합니다.
        
    - 더 정밀한 제어를 위해 **수동 오프셋 관리(Manual offset management)**를 지원합니다.
        

#### Spring Boot 자동 설정의 이점

Spring Boot의 **Auto-Configuration**이 많은 작업을 대신해 줍니다:
- `ConsumerFactory`, `ProducerFactory` Bean 자동 생성.
- 토픽 관리를 위한 Kafka Admin Client.
- 에러 핸들러 및 재시도(Retry) 메커니즘 Bean 설정.
- Spring Actuator를 통한 헬스 체크 및 메트릭.

#### 트랜잭션 관리 전략

**2단계 발행 접근법 (Two-Phase Publishing)**이 가장 견고한 방식입니다:
- 데이터베이스 트랜잭션 커밋 (계좌 잔액 업데이트).
- 별도의 단계에서 이벤트 발행 (보상 로직 포함).
- Saga 패턴을 통한 서비스 간 트랜잭션 일관성 관리.
- Outbox 패턴을 사용하여 Kafka가 일시적으로 다운되더라도 이벤트 발행 보장.

### `@Service` Bean이 있고, 그게 Kafka를 호출해야 한다면... 다른 Spring Bean을 정의해서 그 `@Service` 내부의 트랜잭션을 캡처할 수 있나요?

네, 물론입니다! `@Service` Bean에서 발생하는 트랜잭션을 다른 Spring Bean을 사용해 캡처할 수 있습니다. 주요 구현 패턴은 다음과 같습니다:

#### 방법 1: `@EventListener`를 사용한 Application Events (가장 추천)

`@Service` Bean이 내부적으로 이벤트를 발행하고, 다른 Bean이 이를 리슨하는 방식입니다.

```kotlin
@Service
class BankingService(
    private val applicationEventPublisher: ApplicationEventPublisher, // Spring 내부 이벤트 버스
    private val kafkaTemplate: KafkaTemplate<String, Any>
) {
    fun processTransaction(transaction: Transaction) {
        // 1. 비즈니스 로직 (DB 저장 등)
        saveToDatabase(transaction)

        // 2. 내부 이벤트 발행
        applicationEventPublisher.publishEvent(TransactionEvent(transaction))
    }
}

@Component
class TransactionEventListener(
    private val kafkaTemplate: KafkaTemplate<String, Any>
) {
    @EventListener
    fun handleTransactionEvent(event: TransactionEvent) {
        // 3. 이벤트를 캡처하여 Kafka로 전송
        kafkaTemplate.send("transaction-events", event.transaction)
    }
}
```

#### 방법 2: AOP (Aspect-Oriented Programming) 인터셉션

`@Aspect`를 사용하여 `@Service` Bean의 메서드 호출을 가로채는 방식입니다. 비즈니스 로직과 이벤트 발행을 완전히 분리할 수 있습니다.

#### 방법 3: Transactional Event Listeners

데이터 일관성을 위해, DB 트랜잭션이 커밋된 후에만 이벤트가 발행되도록 보장합니다.

```kotlin
@Component
class TransactionEventCapture {
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    fun handleTransactionCommit(event: TransactionEvent) {
        // DB 트랜잭션이 성공한 후에만 Kafka로 전송
        kafkaTemplate.send("committed-transactions", event)
    }
}
```

#### 방법 4: Spring Boot Kafka 통합 패턴

`event-store-service`가 Kafka 이벤트를 리슨하여 저장하는 방식입니다.

**Application Events (방법 1)** 방식이 결합도를 낮추고 Spring의 내장 기능을 활용하므로 뱅킹 시스템에 가장 깔끔하고 적합합니다.

### 방법 1에서 트랜잭션 동작과 이벤트 캡처가 어떻게 얽혀있는지 설명해 주세요. `event: TransactionEvent`가 핵심인 것 같은데...

맞습니다. `TransactionEvent`가 비즈니스 트랜잭션과 이벤트 캡처를 연결하는 핵심 고리입니다. 각 라인의 의미를 분석해 드리겠습니다.

#### Service Bean (트랜잭션 생산자)

1.  `saveToDatabase(transaction)`: **비즈니스 액션**이 먼저 일어납니다. 실제 뱅킹 로직(DB 업데이트)을 수행합니다.
2.  `TransactionEvent(transaction)`: **이벤트 객체 생성**. 트랜잭션 데이터를 이벤트 객체로 감쌉니다.
3.  `applicationEventPublisher.publishEvent(...)`: **내부 발행**. Spring의 이벤트 버스를 통해 애플리케이션 내부로 이벤트를 방송합니다.

#### Event Listener Bean (이벤트 캡처기)

```kotlin
@Component
class TransactionEventListener(...) {
    @EventListener
    fun handleTransactionEvent(event: TransactionEvent) {
        kafkaTemplate.send("transaction-events", event.transaction)
    }
}
```

1.  `@EventListener`: Spring에게 "이 메서드는 이벤트 핸들러다"라고 알려줍니다.
2.  **파라미터 매칭 (`event: TransactionEvent`)**: Spring은 발행된 이벤트 중 `TransactionEvent` 타입의 객체를 자동으로 이 메서드로 라우팅합니다.
3.  **Kafka 전송**: 캡처된 이벤트를 Kafka를 통해 `event-store-service`로 보냅니다.

#### `TransactionEvent` 정의

이 클래스는 비즈니스 로직과 이벤트 시스템 간의 **계약(Contract)** 역할을 합니다.
```kotlin
data class TransactionEvent(
    val transaction: Transaction, // 실제 데이터
    val timestamp: LocalDateTime, // 메타데이터
    val eventType: String
)
```

#### 전체 흐름 요약

1.  사용자 요청 -> `bankingService.processTransaction()`
2.  비즈니스 로직 실행 (DB 업데이트)
3.  `TransactionEvent` 객체 생성
4.  Spring 내부 이벤트 버스로 발행
5.  Spring이 `TransactionEvent`를 처리할 수 있는 `@EventListener` 메서드를 자동 검색 및 호출
6.  리스너가 이벤트를 캡처하여 Kafka로 전송
7.  외부의 `event-store-service`가 Kafka 메시지 수신

이 패턴은 비즈니스 로직이 Kafka나 이벤트 저장소에 대해 알 필요가 없게 만들어(Loose coupling), 코드를 깔끔하게 유지해줍니다.

### `OperationType`은 어떻게 생겼을까요? 짧게 설명해주세요.

Kotlin Enum Class로 정의됩니다.

```kotlin
enum class OperationType {
    READ,
    WRITE,
    DEPOSIT,    // 입금
    WITHDRAWAL, // 출금
    TRANSFER,   // 이체
    BALANCE_INQUIRY // 잔액 조회
}
```

이벤트 객체에 이를 포함시켜 어떤 작업인지 명시합니다.
`data class TransactionEvent(val op: OperationType, ...)`

### `#event.operationType.name() == 'READ'` 이게 무슨 뜻인지 이해가 안 돼요. `name()`과 `#event`가 뭐죠?

SpEL (Spring Expression Language) 구문입니다.

1.  **`#event`**: 현재 처리 중인 **이벤트 객체 자체**를 가리키는 변수입니다. 리스너 메서드의 파라미터(`event: TransactionEvent`)와 동일한 객체입니다.
2.  **`.operationType`**: 이벤트 객체 내부의 프로퍼티에 접근합니다.
3.  **`.name()`**: Java/Kotlin Enum이 가진 기본 메서드로, Enum 상수의 이름을 **문자열(String)**로 반환합니다. (예: `OperationType.READ` -> `"READ"`)
4.  **`== 'READ'`**: 문자열 비교입니다.

즉, **"이벤트의 작업 타입 이름이 'READ'일 때만 이 리스너를 실행하라"**는 조건문입니다.

### `#event`를 호출한다는 건, 그 `TransactionEvent` 타입의 인스턴스를 참조한다는 뜻인가요?

네, 정확합니다!

`#event`는 `@EventListener` 메서드의 파라미터로 전달될 **바로 그 `TransactionEvent` 객체 인스턴스**를 가리킵니다.
조건식(Condition)을 평가하는 시점에 해당 객체에 접근하여 필터링 로직을 수행할 수 있게 해줍니다.

### SpEL에 대해 좀 더 알려주세요.

**Spring Expression Language (SpEL)**은 런타임에 객체 그래프를 조회하고 조작할 수 있는 강력한 표현 언어입니다.

#### 주요 특징 및 용도

1.  **문법:** `#{expression}` (설정 파일 등) 또는 어노테이션 속성 내에서 사용.
2.  **리터럴:** `'Hello'`, `true`, `100` 등.
3.  **객체 프로퍼티 접근:** `user.name` (getter 호출).
4.  **메서드 호출:** `person.getAge()`.
5.  **정적 메서드/상수 접근:** `T(java.lang.Math).random()`. `T()` 연산자를 사용합니다.
6.  **변수:** `#variableName` 형태로 컨텍스트 변수 접근 (예: `#event`).
7.  **조건부 이벤트 처리:** `@EventListener(condition = "...")`에서 이벤트를 필터링할 때 매우 유용합니다.

### 정리해봅시다. Kafka로 트랜잭션을 할 때, Event Sourcing을 사용해 트랜잭션 정보를 저장하고, 이는 이벤트 핸들러를 정의하여 MongoDB에 저장되는 방식... 맞나요?

네, **완벽하게 이해하셨습니다!**

#### 전체 아키텍처 흐름 확인

1.  **비즈니스 트랜잭션 발생:** 서비스에서 이체, 입금 등의 로직 수행.
2.  **Event Sourcing 패턴:** 상태를 덮어쓰는 대신, 트랜잭션을 **불변의 이벤트**로 캡처.
3.  **Kafka 통합:** 이벤트를 Kafka 토픽으로 발행 (안정성, 분산 처리).
4.  **Event Handler 처리:** `event-store-service`의 핸들러(`@KafkaListener`)가 이벤트를 소비.
5.  **MongoDB 저장:** 핸들러가 이벤트 정보를 MongoDB에 영구적인 문서(Document)로 저장.

이 구조 덕분에 데이터 손실 없이 모든 이력을 저장(Event Sourcing)하고, 유연하게 조회(MongoDB)할 수 있으며, 시스템 간 결합도를 낮출 수(Kafka) 있습니다.

---

### 요약 (Summary)

**Event Sourcing 패턴**

- 변경 내역을 불변의 이벤트 시퀀스로 저장합니다.
- 핵심 원칙: 불변 로그, 상태 재구성, 감사 추적, 타임 트래블.
- CQRS와 결합하여 쓰기(Command)와 읽기(Query)를 분리하는 데 사용됩니다.

**시스템 아키텍처: Kafka & Spring을 활용한 Event Sourcing**

- **트랜잭션 흐름:**
    1. `@Service`가 트랜잭션 수행.
    2. 트랜잭션을 나타내는 이벤트 생성.
    3. Kafka 토픽으로 이벤트 발행.
    4. `event-store-service`가 이벤트 소비.
    5. MongoDB에 이벤트 저장.

**Spring 이벤트 캡처 구현 패턴**

- **Application Events (추천):**
    - `@Service`는 `ApplicationEventPublisher`로 내부 이벤트 발행.
    - 별도의 `@Component`가 `@EventListener`로 이를 잡아 Kafka로 전송.
    - 비즈니스 로직과 메시징 인프라의 완벽한 분리.
- **Transactional Event Listeners:**
    - `@TransactionalEventListener(phase = AFTER_COMMIT)`을 사용하여 DB 커밋 성공 후에만 이벤트 발행 보장.

**SpEL (Spring Expression Language)**

- 런타임 객체 조작 언어.
- **이벤트 리스너 활용:** `@EventListener`의 `condition` 속성에서 `#event` 변수를 통해 이벤트 내용을 검사하고 필터링하는 데 사용.
- Enum 비교 시 `.name()` 메서드를 사용하여 안전하게 문자열 비교 가능.