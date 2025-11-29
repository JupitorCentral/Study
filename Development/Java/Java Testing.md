---
tags:
  - Java/Testing
---

## JUnit 5

Java 단위 테스트 프레임워크.

### Key Annotations
- **@Test**: 테스트 메서드임을 명시.
- **@BeforeEach / @AfterEach**: 각 테스트 전/후 실행.
- **@BeforeAll / @AfterAll**: 전체 테스트 전/후 1회 실행 (static).
- **@DisplayName**: 테스트 이름 지정.
- **@Nested**: 내부 클래스로 테스트 그룹화 (BDD 스타일 계층 구조).
- **@Tag**: 테스트 필터링 (예: "slow", "integration").

### Assertions
- `assertEquals(expected, actual)`
- `assertTrue(condition)`
- `assertNotNull(object)`
- `assertThrows(Exception.class, executable)`: 예외 발생 검증.

### Conditional Execution
- `@EnabledOnOs`, `@EnabledOnJre`
- `@EnabledIf`, `@DisabledIf` (Spring SpEL 사용 가능).
