---
tags:
  - spring
  - transaction
  - kotlin
---

### `@Transactional` 어노테이션은 정확히 무슨 일을 하나요? 특히 예외 발생과 롤백에 대해서요.

네, 맞습니다. `@Transactional`은 예외가 발생했을 때 데이터베이스 롤백을 자동으로 처리해 줍니다. 뱅킹 프로젝트 관점에서 더 자세히 설명해 드리겠습니다.

#### @Transactional의 작동 원리

메서드에 `@Transactional`을 붙이면, Spring은 해당 메서드를 감싸는 **프록시(Proxy)**를 생성하여 트랜잭션 관리를 수행합니다.

```kotlin
@Transactional
fun transferMoney(fromAccount: Long, toAccount: Long, amount: BigDecimal) {
    // 이 블록 안의 모든 DB 작업은 하나의 트랜잭션으로 묶입니다.
    accountService.debit(fromAccount, amount)
    accountService.credit(toAccount, amount)
    // 예외가 발생하면? 전체 작업이 취소(Rollback)됩니다.
}
```

내부적으로는 다음과 같이 동작합니다:
1. `connection.setAutoCommit(false)`: 트랜잭션 시작
2. 비즈니스 로직 실행
3. 성공 시 `connection.commit()`
4. 예외 발생 시 `connection.rollback()`

#### 예외 처리 및 롤백 규칙

**기본 동작 (Default Behavior)**
- **Unchecked 예외** (`RuntimeException`, `Error` 등): **자동 롤백**됩니다.
- **Checked 예외** (`Exception`, `IOException` 등): **롤백되지 않습니다.**

**커스텀 롤백 설정**
이 기본 동작을 변경하여 모든 예외에 대해 롤백하도록 설정할 수 있습니다.

```kotlin
// 모든 Exception에 대해 롤백 (Checked 예외 포함)
@Transactional(rollbackFor = [SQLException::class, Exception::class])
fun riskyOperation() { ... }
```

#### Kotlin 사용 시 주의사항

**`open` 키워드**
Spring의 프록시 메커니즘(CGLIB)이 작동하려면 클래스와 메서드가 상속 가능해야 합니다. 하지만 Kotlin의 클래스와 메서드는 기본적으로 `final`입니다.
따라서 Spring AOP가 적용될 클래스나 메서드에는 `open`을 붙여야 합니다. (단, `kotlin-spring` 플러그인을 쓰면 자동으로 처리됩니다.)

```kotlin
@Service
open class BankService { // open 필요
    @Transactional
    open fun processTransaction() { ... } // open 필요
}
```

**Coroutines 제한**
`@Transactional`은 기본적으로 스레드 로컬(ThreadLocal)을 사용하므로, 스레드를 넘나드는 **Kotlin Coroutines와는 호환되지 않습니다.** 코루틴을 쓴다면 R2DBC나 코루틴 전용 트랜잭션 관리자를 사용해야 합니다.

#### 주요 설정 속성

| 속성 | 설명 | 예시 |
|---|---|---|
| `rollbackFor` | 롤백을 유발할 예외 타입 지정 | `rollbackFor = [Exception::class]` |
| `readOnly` | 읽기 전용으로 최적화 (쓰기 방지) | `readOnly = true` |
| `propagation` | 트랜잭션 전파 방식 설정 (기존 트랜잭션 합류 등) | `propagation = REQUIRES_NEW` |
| `isolation` | 격리 수준 설정 (동시성 제어) | `isolation = READ_COMMITTED` |

#### 뱅킹 프로젝트를 위한 권장 사항

금융 거래의 무결성은 매우 중요하므로 다음을 고려하세요:

- **포괄적 롤백:** `rollbackFor = [Exception::class]`를 사용하여 Checked 예외가 발생해도 안전하게 롤백되도록 하세요.
- **서비스 계층 적용:** 여러 DB 작업이 묶이는 `@Service` 계층 메서드에 적용하세요.
- **테스트:** 예외 발생 시 실제로 데이터가 롤백되는지 테스트 코드로 검증하세요.

---

### 요약 (Summary)

**핵심 기능**
- 메서드 실행을 DB 트랜잭션으로 감쌉니다.
- 성공하면 커밋, 실패하면 롤백을 자동화합니다.

**롤백 규칙**
- **기본:** `RuntimeException`만 롤백.
- **권장:** `rollbackFor = [Exception::class]`를 사용하여 모든 예외 상황에 대비.

**Kotlin 고려사항**
- **`open`:** 프록시 생성을 위해 클래스/메서드 개방 필요 (플러그인 사용 시 자동).
- **Coroutines:** 기본 `@Transactional`은 코루틴과 함께 쓸 수 없음.

**주요 속성**
- `readOnly`: 조회 성능 최적화.
- `propagation`: 트랜잭션 전파 규칙 (새로 만들지, 기존 것 쓸지).
- `isolation`: 데이터 격리 수준 (더티 리드 방지 등).