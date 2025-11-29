---
tags:
  - architecture
  - cqrs
  - kafka
  - spring
---

### CQRS가 도대체 뭔가요? RESTful API + MVC 패턴과 비교해서 설명해 주세요.

**CQRS (Command Query Responsibility Segregation)**는 시스템의 **상태를 변경하는 작업(Command, 쓰기)**과 **데이터를 조회하는 작업(Query, 읽기)**을 서로 다른 모델로 분리하는 아키텍처 패턴입니다. 반면, 전통적인 RESTful API + MVC 패턴은 보통 하나의 통합된 모델로 읽기와 쓰기를 모두 처리합니다.

#### 주요 차이점 (Key Differences)

**전통적인 MVC + REST**

- 하나의 모델(Entity)이 읽기와 쓰기를 모두 담당합니다.
- 모든 작업에 동일한 데이터베이스 스키마를 사용합니다.
- Controller가 GET(조회)과 POST/PUT/DELETE(변경)를 모두 처리합니다.
- 단순하고 직관적입니다.

**CQRS**

- **명령(Command, 쓰기):** 쓰기 전용 모델을 사용하며, 별도의 쓰기 DB를 가질 수 있습니다.
- **조회(Query, 읽기):** 읽기 전용 모델(최적화된 뷰)을 사용하며, 별도의 읽기 DB를 가질 수 있습니다.
- Command Handler가 쓰기를, Query Handler가 읽기를 처리합니다.
- 쓰기와 읽기 모델 간의 데이터 동기화를 위해 **Event Sourcing**을 자주 사용합니다.

#### 뱅킹 시스템 예시 (Kotlin)

**MVC + REST 접근법**

```kotlin
@RestController
class AccountController {
    // 읽기도 같은 서비스/모델 사용
    @GetMapping("/accounts/{id}")
    fun getAccount(@PathVariable id: String): Account {
        return accountService.findById(id)
    }
    
    // 쓰기도 같은 서비스/모델 사용
    @PostMapping("/accounts/{id}/transfer")
    fun transfer(...) {
        accountService.transfer(request)
    }
}
```

**CQRS 접근법**

```kotlin
// Command Side (쓰기)
@Component
class TransferCommandHandler {
    fun handle(command: TransferCommand) {
        // 쓰기 DB에 저장
        // 이벤트 발행
    }
}

// Query Side (읽기)
@RestController
class AccountQueryController {
    // 읽기 전용 최적화 모델 사용
    @GetMapping("/accounts/{id}")
    fun getAccount(@PathVariable id: String): AccountView {
        return accountQueryService.getAccountView(id)
    }
}
```

#### 언제 사용해야 할까요?

**MVC + REST 사용:**
- 단순한 CRUD 작업 위주인 경우.
- 소~중규모 애플리케이션.
- 팀이 단순성을 선호할 때.

**CQRS 사용:**
- 비즈니스 로직이 매우 복잡할 때.
- 읽기와 쓰기의 성능 요구사항이 크게 다를 때 (예: 조회는 엄청 많은데 쓰기는 적음).
- 감사(Audit) 추적이나 히스토리 관리가 중요할 때.

---

### 이벤트(Event)를 캡처하는 게 CQRS의 핵심인가요?

이벤트는 CQRS에서 **매우 중요**하지만, 핵심은 단순히 이벤트를 잡는 게 아니라 **명령(Command)과 조회(Query) 사이를 동기화하는 '이벤트 흐름'**입니다.

#### CQRS에서의 이벤트 역할

흐름: **Command Side → Events → Query Side**

1.  **명령(Command):** 쓰기 작업이 성공하면 **도메인 이벤트**를 생성합니다.
2.  **이벤트(Event):** "무슨 일이 일어났는지"에 대한 정보를 담습니다.
3.  **조회(Query):** 이벤트를 소비(Consume)하여 별도의 읽기 모델(Read Model)을 업데이트합니다.

#### Kotlin 예시

```kotlin
// Command Side: 이벤트 생성 및 발행
class TransferCommandHandler {
    fun handle(command: TransferCommand) {
        // 비즈니스 로직 수행
        val event = MoneyTransferredEvent(...)
        eventPublisher.publish(event)
    }
}

// Query Side: 이벤트 소비 및 뷰 업데이트
@EventHandler
class AccountViewUpdater {
    fun on(event: MoneyTransferredEvent) {
        // 읽기 모델 업데이트
        accountViewRepository.updateBalance(...)
    }
}
```

#### 핵심 포인트

- 이벤트는 분리된 두 모델(쓰기/읽기)을 **동기화**하는 수단입니다.
- 이벤트 덕분에 두 모델 간의 **결과적 일관성(Eventual Consistency)**이 가능해집니다.
- **Event Sourcing**은 CQRS와 함께 쓰이는 선택적인 구현 방식입니다.

---

### 흐름이 `유저 요청 -> Command Event -> Event Sourcing -> Query Event` 인가요?

비슷하지만 용어가 조금 다릅니다. "Command Event"나 "Query Event"라는 용어보다는 다음과 같은 흐름이 정확합니다.

#### 정확한 CQRS 이벤트 흐름

**유저 요청(Request) → 명령(Command) → 도메인 이벤트(Domain Event) → 조회 모델 업데이트(Query Model Update)**

1.  **유저 요청:** `POST /transfer { ... }`
2.  **명령 처리:** 요청이 `TransferCommand`로 변환되어 핸들러가 처리.
3.  **도메인 이벤트 생성:** 성공 시 `MoneyTransferredEvent` 생성. (이것이 Event Sourcing의 대상)
4.  **조회 모델 소비:** `AccountViewUpdater` 같은 리스너가 이벤트를 받아 읽기 DB를 갱신.

#### 수정 사항

- 명령(Command) 처리 후 생성되는 것은 **도메인 이벤트**입니다.
- 조회(Query) 측은 이벤트를 생성하는 게 아니라, 이벤트를 **소비**합니다.
- **Event Sourcing**은 이 과정을 거치며 이벤트를 영구 저장하는 전략을 말합니다.

---

### Command 쪽이 생산자(Producer), Query 쪽이 소비자(Consumer) 같은 건가요? OS의 이벤트 큐처럼요?

네, 아주 훌륭한 비유입니다! **Command Side는 생산자(Producer)**이고, **Query Side는 소비자(Consumer)** 역할을 합니다.

#### CQRS 생산자-소비자 패턴

- **Command Side (Producer):**
    - 비즈니스 명령을 처리.
    - 성공 시 도메인 이벤트를 생성하여 스트림/큐에 발행(Publish).
- **Query Side (Consumer):**
    - 이벤트를 구독(Subscribe).
    - 이벤트를 기반으로 읽기 전용 뷰(Projections)를 업데이트.
    - 사용자에게 빠른 조회를 제공.

#### OS 이벤트 큐와의 차이점

- **비즈니스 문맥:** OS 이벤트는 저수준(클릭, 키 입력)이지만, 도메인 이벤트는 비즈니스 의미(송금됨, 계좌 생성됨)를 담습니다.
- **결과적 일관성:** 즉시 처리되기보다, 약간의 시차를 두고 데이터가 일치해지는 것을 허용합니다.

---

### 송금 예제 코드를 보니 Spring Event Capture Handler를 쓰는 것 같던데 맞나요?

네, 맞습니다. Spring의 `@EventListener`와 `ApplicationEventPublisher`는 CQRS의 이벤트 처리 부분을 구현하기에 완벽한 도구입니다.

#### Spring Event Handler 예시

**Command Side (이벤트 발행)**

```kotlin
@Service
class TransferCommandHandler(...) {
    @Transactional
    fun handle(command: TransferCommand) {
        // 1. 비즈니스 로직 (DB 반영)
        fromAccount.withdraw(...)
        toAccount.deposit(...)
        
        // 2. 트랜잭션 성공 후 도메인 이벤트 발행
        val event = MoneyTransferredEvent(...)
        applicationEventPublisher.publishEvent(event)
    }
}
```

**Query Side (이벤트 소비)**

```kotlin
@Component
class AccountViewProjector(...) {
    @EventListener
    @Async // 비동기로 실행하여 Command Side를 차단하지 않음
    fun on(event: MoneyTransferredEvent) {
        // 읽기 모델(예: 조회용 테이블) 업데이트
        accountViewRepository.updateBalance(...)
    }
}
```

---

### 그럼 이 패턴을 쓰면 RESTful CRUD처럼 동기식으로 결과를 바로 못 받나요?

정확합니다! 이것이 CQRS와 전통적인 RESTful CRUD의 가장 큰 차이점이자 트레이드오프입니다. 흐름이 근본적으로 **비동기(Asynchronous)**입니다.

#### 비교: RESTful CRUD vs. CQRS

**RESTful CRUD (동기식)**
- 클라이언트가 요청을 보내면, DB 업데이트가 끝날 때까지 기다렸다가 최종 결과를 받습니다.
- `return TransferResponse(balance, "완료")`

**CQRS (비동기식)**
- 클라이언트가 명령을 보내면, 서버는 "요청 접수됨"만 응답하고 실제 처리는 백그라운드에서 합니다.
- `return ResponseEntity.accepted().body("처리 중")`

|구분|RESTful CRUD|CQRS|
|---|---|---|
|**실행 방식**|동기 (Synchronous)|비동기 (Asynchronous)|
|**일관성**|즉시 일관성 (Immediate)|결과적 일관성 (Eventual)|
|**흐름**|요청 → 처리 → 완료 응답|요청 → 명령 → 이벤트 → 뷰 업데이트|
|**스레딩**|Blocking (기다림)|Non-blocking (바로 리턴)|

---

### 그럼 서버는 유저에게 거래가 끝났다는 걸 어떻게 알려주나요?

초기 응답은 "접수됨(202 Accepted)"일 뿐이므로, 최종 결과를 알리기 위한 별도의 메커니즘이 필요합니다.

#### 완료 알림 전략

1.  **클라이언트 폴링 (Polling):** 클라이언트가 주기적으로 `GET /status`를 호출해 확인. (구현 쉬움, 서버 부하)
2.  **웹소켓 (WebSocket / STOMP):** 클라이언트와 연결을 유지하고, 완료 시 서버가 메시지 전송. (실시간, 구현 복잡)
3.  **Server-Sent Events (SSE):** 서버에서 클라이언트로 단방향 스트림 전송. (웹소켓보다 단순)
4.  **푸시 알림 (FCM 등):** 모바일 앱인 경우 푸시 메시지 전송.
5.  **이메일/SMS:** 비동기 작업 완료 후 알림 발송.

---

### 이제 Kafka의 역할을 알겠네요. Command Side의 이벤트를 큐에 저장해서 Query Side가 처리하게 하고, 상태를 관리하는 거군요?

네, 맞습니다. 분산 CQRS 시스템에서 **Kafka는 견고하고 확장 가능한 이벤트 백본(Backbone)** 역할을 합니다.

- **Command Side:** 이벤트를 Kafka 토픽에 **발행(Publish)**.
- **Kafka:** 이벤트를 순서대로 **영구 저장**.
- **Query Side:** 토픽을 **구독(Consume)**하여 읽기 모델 업데이트.

하지만 한 가지 명확히 해야 할 점은: Kafka는 비즈니스적인 상태("완료", "실패")를 관리하지 않습니다. Kafka는 **메시지의 전달 상태(Offset)**만 관리합니다.

#### Kafka가 관리하는 것

- **순서 및 내구성:** 이벤트가 순서대로 저장되고 유실되지 않음을 보장.
- **보존(Retention):** 설정된 기간 동안 이벤트를 저장 (나중에 리플레이 가능).
- **오프셋(Consumer Offsets):** 컨슈머가 어디까지 읽었는지를 추적.

비즈니스 로직 상의 성공/실패 여부는 애플리케이션이 판단해야 합니다.

---

### 시스템이 죽었다가 재부팅되면, Kafka가 이벤트를 저장하고 있으니까 로그에서 이어서 처리하나요?

네, 정확합니다! 이것이 Kafka를 이벤트 저장소로 사용할 때의 가장 강력한 장점입니다. **Kafka의 내구성과 오프셋 추적 덕분에 장애 복구가 가능합니다.**

#### 장애 복구 프로세스

1.  Query 서비스가 죽었다가 재시작합니다.
2.  동일한 '컨슈머 그룹'으로 Kafka에 다시 연결합니다.
3.  Kafka는 이 그룹이 마지막으로 **어디까지 읽었는지(Offset)** 기억하고 있습니다.
4.  서비스는 중단된 지점부터 자동으로 이벤트를 다시 읽어옵니다. 데이터 유실이 없습니다.

---

### Kafka는 비즈니스 상태를 모른다면서요. 그럼 Spring은 Query Side 작업이 끝났는지 어떻게 알죠?

맞습니다. Kafka는 배달부일 뿐입니다. 메시지를 잘 처리했는지 Kafka에게 알려주는 건 **Spring 애플리케이션의 몫**입니다. 이를 **Consumer Acknowledgment (ACK)**라고 합니다.

#### 처리 완료 추적 (Manual Acknowledgment)

트랜잭션 안전성을 위해 Spring Kafka에서 **수동 ACK(Manual Acknowledgment)**를 사용하는 것이 좋습니다.

1.  리스너가 메시지를 받습니다.
2.  비즈니스 로직(DB 업데이트)을 수행합니다.
3.  성공하면 `acknowledgment.acknowledge()`를 호출합니다. -> Kafka: "오케이, 오프셋 넘긴다."
4.  실패하면 `nack()`하거나 아무것도 하지 않습니다. -> Kafka: "다시 보내줄게" (혹은 재시도 처리).

```kotlin
@KafkaListener(topics = ["transfer-events"])
fun handleEvent(event: MoneyTransferredEvent, ack: Acknowledgment) {
    try {
        // 1. 읽기 모델 업데이트
        accountViewRepository.updateBalance(...)
        
        // 2. 성공 시에만 Kafka에 확인 도장 찍기
        ack.acknowledge()
    } catch (e: Exception) {
        // 3. 실패 시 재시도 요청
        ack.nack(Duration.ofSeconds(5))
    }
}
```

---

### 요약 (Summary)

**CQRS 기본**
- **Command(쓰기)와 Query(읽기)의 분리:** 성능, 확장성, 복잡성 관리를 위해 모델을 나눕니다.
- **비동기 흐름:** `요청 -> 명령 -> 이벤트 -> 뷰 업데이트`의 흐름을 가집니다.
- **결과적 일관성:** 읽기 데이터가 실시간이 아닐 수 있음을 전제합니다.

**Kafka와 Spring의 역할**
- **Kafka:** 이벤트를 영구 저장하고 전달하는 신뢰성 있는 **저장소이자 파이프라인**. 장애 시 복구를 보장합니다.
- **Spring:** 이벤트를 발행하고 소비하는 **애플리케이션 로직**을 담당합니다.
- **수동 ACK:** Spring이 비즈니스 로직 성공 여부를 Kafka에 알려주어 메시지 유실을 방지합니다.

**사용자 경험**
- 작업이 비동기로 처리되므로, **폴링, 웹소켓, 푸시 알림** 등을 통해 사용자에게 완료를 알려야 합니다.