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
