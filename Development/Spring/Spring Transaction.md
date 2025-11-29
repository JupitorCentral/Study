---
tags:
  - Spring/Transaction
---

## @Transactional

Spring에서 선언적 트랜잭션 관리를 위해 사용하는 어노테이션.
메서드나 클래스에 붙여 트랜잭션의 경계를 설정함.

### 1. 동작 원리 (AOP)
`@EnableTransactionManagement`가 활성화되면:
1. **Advisor** (`BeanFactoryTransactionAttributeSourceAdvisor`)가 `@Transactional` 메서드를 감지.
2. **Proxy**를 생성하여 해당 메서드 호출을 가로챔.
3. **Interceptor** (`TransactionInterceptor`)가 실행됨.
    - `TransactionManager`를 통해 트랜잭션 **시작 (Begin)**.
    - 실제 메서드 실행.
    - 성공 시 **커밋 (Commit)**.
    - 예외 발생 시 **롤백 (Rollback)**.

### 2. Transaction Attributes

#### readOnly = true
- **읽기 전용** 트랜잭션임을 명시.
- **최적화**:
    - Hibernate: 스냅샷 생성 안 함, Dirty Checking(변경 감지) 안 함 -> 성능 향상.
    - DB: 일부 DB는 읽기 전용 트랜잭션에 대해 리소스 최적화.
- **주의**: 쓰기 작업(Insert, Update, Delete) 시 예외 발생하거나 무시됨.

#### ACID 속성
- **Atomicity (원자성)**: All or Nothing.
- **Consistency (일관성)**: 트랜잭션 전후 데이터 무결성 유지.
- **Isolation (격리성)**: 동시에 실행되는 트랜잭션끼리 영향 없음.
- **Durability (지속성)**: 성공 시 영구 저장.

#### Rollback Rules
- 기본적으로 **Unchecked Exception (RuntimeException, Error)** 발생 시 롤백.
- **Checked Exception**은 롤백 안 함 (개발자가 처리해야 한다고 간주).
- 설정으로 변경 가능: `@Transactional(rollbackFor = Exception.class)`

### 3. Transaction Propagation (전파)
- **REQUIRED (기본값)**: 부모 트랜잭션이 있으면 참여, 없으면 새로 생성.
- **REQUIRES_NEW**: 항상 새로운 트랜잭션 생성 (부모는 일시 중지).
- **SUPPORTS**: 있으면 참여, 없으면 트랜잭션 없이 실행.
- **MANDATORY**: 반드시 부모 트랜잭션이 있어야 함 (없으면 예외).