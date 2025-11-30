---
tags:
  - Spring/Testing
---

## Spring Boot Testing

### 1. @SpringBootTest
- 통합 테스트용. 실제 Spring ApplicationContext를 로드함.
- 모든 빈을 등록하므로 무겁지만, 실제 환경과 가장 유사함.

### 2. Mocking (Mockito)
- **@Mock**: Mockito의 Mock 객체 생성 (Spring Context와 무관). 단위 테스트용.
- **@MockBean**: Spring Context에 Mock 객체를 빈으로 등록 (기존 빈을 대체). 통합 테스트용.
- **@InjectMocks**: `@Mock`으로 만든 객체들을 주입받을 대상 객체.

### 3. MockMvc
- 웹 계층(Controller) 테스트용 도구.
- 서블릿 컨테이너를 띄우지 않고 HTTP 요청/응답을 모의(Mocking)하여 테스트.

### 4. Database Testing
- **H2 Database**: 인메모리 DB. 테스트용으로 간편하지만 프로덕션 DB(MySQL 등)와 문법/동작 차이 존재.
- **Testcontainers**: Docker를 이용해 실제 DB 환경을 띄워 테스트. 신뢰성 높음.

---
## Test Environment Configuration Example

### Spring Boot Test Properties (TestContainer & H2/Redis)

```properties
# ============================================================================  
# Spring Boot 테스트 환경 설정  
# ============================================================================  
  
# ============================================================================  
# 애플리케이션 이름 설정  
# ============================================================================  
spring.application.name=PulseTicket-Test-container  
  
# ============================================================================  
# DataSource 설정  
# spring.datasource.* 값은 컨테이너가 채움  
# ============================================================================  
  
# ============================================================================  
# JPA/Hibernate 설정  
# ============================================================================  
spring.jpa.hibernate.ddl-auto=validate  
spring.jpa.show-sql=true  
spring.jpa.properties.hibernate.format_sql=true  
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect  
  
# ============================================================================  
# Redis/Session 설정  
# ============================================================================  
spring.data.redis.host=localhost  
spring.data.redis.port=${TEST_REDIS_PORT:6379}  
spring.data.redis.password=${TEST_REDIS_PASSWORD:testpass}  
spring.data.redis.timeout=2000ms  
spring.session.redis.flush-mode=on_save  
spring.session.redis.namespace=spring:session:test  
server.servlet.session.timeout=30m  
  
# ============================================================================  
# 서버 포트 설정  
# ============================================================================  
server.port=0
```

### Configuration Analysis

1. **Application Name**
   `spring.application.name=PulseTicket-Test-container`
   - 테스트 환경에서 실행되는 애플리케이션의 이름을 지정.
   - 로그나 모니터링에서 애플리케이션을 구분하기 위해 사용됨.
   - 프로덕션과 테스트 환경을 구분하기 위해 `-Test-container` 접미사를 붙임.

2. **JPA/Hibernate 설정**
   `spring.jpa.hibernate.ddl-auto=validate`
   - `validate`: DB 스키마와 엔티티가 일치하는지만 검증함.
   - `create/update`가 아닌 `validate`를 쓴 이유: 테스트 컨테이너에서 이미 마이그레이션 스크립트로 스키마를 생성했을 것이기 때문임.

   `spring.jpa.show-sql=true`, `spring.jpa.properties.hibernate.format_sql=true`
   - 실행되는 SQL을 콘솔에 출력하고 포맷팅해서 보여주는 설정.
   - 테스트 중 어떤 쿼리가 실행되는지 디버깅하기 위해 필요함.

   `spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect`
   - PostgreSQL 데이터베이스를 사용한다고 Hibernate에 명시.

3. **Redis/Session 설정**
   `spring.data.redis.host=localhost`
   `spring.data.redis.port=${TEST_CONTAINER_REDIS_PORT:26381}`
   - TestContainer로 띄운 Redis에 연결하기 위한 설정.
   - 포트는 환경변수로 주입받되, 없으면 기본값 26381을 사용. TestContainer가 랜덤 포트를 할당하면 `TEST_CONTAINER_REDIS_PORT`로 주입.

   `spring.session.redis.flush-mode=on_save`
   - `on_save`: 세션 데이터가 변경되면 즉시 Redis에 저장.
   `spring.session.redis.namespace=spring:session:test`
   - Redis 키에 `spring:session:test` 접두사를 붙여서 다른 환경과 구분.

4. **서버 포트 설정**
   `server.port=0`
   - OS가 사용 가능한 랜덤 포트를 자동으로 할당하도록 하는 설정.
   - 테스트를 병렬로 실행할 때 포트 충돌 방지.

---
## Spring Test Annotation Comparison

### @SpringBootTest vs @TestContainer

**@SpringBootTest**
- Spring Application Context를 로드하는 어노테이션.
- 모든 `@Component`, `@Service`, `@Repository`, `@Controller` 빈들을 생성하고 주입.
- 전체 애플리케이션 통합 테스트(Integration Test) 가능.

**@TestContainer**
1. **Container Lifecycle 자동 관리**: `@Container` 어노테이션이 붙은 필드들을 찾아 테스트 시작 전 구동, 종료 후 삭제.
2. **실제 외부 의존성을 Docker로 구동**: Mock이나 H2 대신 실제 PostgreSQL, Redis 등을 사용.

| 어노테이션 | 해결하는 문제 | 핵심 기능 |
| :--- | :--- | :--- |
| `@SpringBootTest` | "Spring Bean들을 어떻게 준비하지?" | Application Context 로드 |
| `@Testcontainers` | "외부 시스템(DB 등)을 어떻게 준비하지?" | Docker Container 관리 |

### H2만 쓰려면?
- `@DataJpaTest`면 충분함 (인메모리 DB 사용 시).
- 실제 DB를 쓰려면 `@SpringBootTest` + `@Testcontainers` 조합이 필요.

### @DataJpaTest 상세
- JPA 관련 컴포넌트들만 로드하는 "Test Slice".
- **로드되는 것들**: `@Repository`, `EntityManager`, `DataSource` (H2), `TestEntityManager`.
- **로드되지 않는 것들**: `@Service`, `@Controller`, `@Component`.

**Service 계층 테스트 방법**:
1. **Mock 사용 (Unit Test)**: `@ExtendWith(MockitoExtension.class)` + `@Mock` Repository.
2. **@ComponentScan 추가**: `@DataJpaTest`에 `@ComponentScan`으로 Service 패키지 포함 (비권장).
3. **@SpringBootTest 사용**: 전체 통합 테스트.

---
## Redis Testing Strategies

### 1. Embedded Redis vs TestContainers

| 상황 | 선택 | 이유 |
| :--- | :--- | :--- |
| 빠른 로컬 개발 테스트 | Embedded Redis | 간단하고 빠름 |
| 운영환경 검증 필요 | TestContainers | 실제 Redis와 동일 |
| Redis 특정 기능 테스트 | TestContainers | Pub/Sub, Stream 등 모든 기능 지원 |
| CI/CD 파이프라인 | TestContainers | 일관된 환경 보장 |
| Docker 설치 불가능 | Embedded Redis | Docker 불필요 |

### 2. Performance Testing (Load Test)
`@SpringBootTest` + `TestContainers` 조합을 권장. H2는 운영환경과 성능 특성이 다르기 때문.

```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@Testcontainers
class LoadTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");
    
    @Container
    static GenericContainer<?> redis = new GenericContainer<>("redis:7-alpine").withExposedPorts(6379);
    
    @Test
    void performanceTest() {
        // 실제 DB/Redis 사용하며 부하 테스트 진행
    }
}
```

---
## JUnit 5 Basics

### Core Functions
1. **Test Runner**: 테스트 메소드 실행, 결과 수집.
2. **Assertions**: `assertEquals`, `assertTrue`, `assertNotNull` 등 검증 메소드 제공.
3. **Lifecycle Management**: `@BeforeAll`, `@BeforeEach`, `@AfterEach`, `@AfterAll`.

### JUnit vs Spring Boot Test
JUnit은 기반(Foundation) 프레임워크이고, Spring Boot Test는 그 위에서 Spring Context를 로드해주는 확장 기능(`SpringExtension`)입니다.

**실행 순서 예시**:
1. **JUnit**: `@Test` 메소드 탐색.
2. **Testcontainers**: Docker 컨테이너 시작.
3. **Spring**: ApplicationContext 로드.
4. **JUnit**: `@BeforeEach` -> 테스트 메소드 -> `assertEquals` -> `@AfterEach`.

### Gradle Test Execution
Gradle로 테스트 실행 시:
`Gradle Daemon` -> `Worker Process` -> `JUnit Platform` 순으로 실행됨.

- **IDE 실행**: JUnit Runner가 직접 실행 (빠름).
- **Gradle 실행 (`./gradlew test`)**: 전체 빌드 프로세스 포함 (느림, CI/CD용).

---
## H2 + Embedded Redis Configuration Tips

### H2 In-Memory vs File Mode
- **In-Memory (`jdbc:h2:mem:testdb`)**: 빠르고 격리됨. 데이터 확인 어려움.
- **File Mode (`jdbc:h2:file:./build/h2db`)**: 느리지만 H2 Console로 디버깅 가능.

### Embedded Redis Configuration Example
Embedded Redis는 Spring이 자동으로 띄워주지 않으므로 `@TestConfiguration`으로 수동 제어 필요.

```java
@TestConfiguration
public class EmbeddedRedisConfig {
    @PostConstruct
    public void startRedis() {
        redisServer.start(); // 예: 6370 포트 (기본 6379 충돌 방지)
    }

    @PreDestroy
    public void stopRedis() {
        redisServer.stop();
    }
}
```