---
tags:
  - DevOps/Docker
---

## Docker Basics

### Docker vs VM
- **VM**: 하드웨어 가상화 + Guest OS 포함 (무거움).
- **Docker**: 호스트 OS 커널 공유 + 프로세스 격리 (가벼움).

### Docker Compose
- 여러 컨테이너를 정의하고 실행하기 위한 도구 (`docker-compose.yml`).
- 서비스, 네트워크, 볼륨 등을 정의하여 한 번에 관리 (`up`, `down`).

### Volume
- 컨테이너 데이터의 영속성(Persistence)을 보장하기 위한 저장소.
- 컨테이너가 삭제되어도 볼륨에 저장된 데이터는 유지됨 (DB 데이터 보존에 필수).
- `postgres_data:/var/lib/postgresql/data` 와 같이 마운트.


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

```json
1. Application Name (5-6번 줄)

  spring.application.name=PulseTicket-Test-container
  
  - 테스트 환경에서 실행되는 애플리케이션의 이름을 지정하는 것임
  - 로그나 모니터링에서 애플리케이션을 구분하기 위해 사용됨
  - 프로덕션과 테스트 환경을 구분하기 위해 -Test-container 접미사를 붙인 것임

  2. JPA/Hibernate 설정 (8-13번 줄)

  spring.jpa.hibernate.ddl-auto=validate
  - validate: DB 스키마와 엔티티가 일치하는지만 검증함
  - create/update가 아닌 validate를 쓴 이유: 테스트 컨테이너에서 이미 마이그레이션 스크립트로 스키마를 생성했을 것이기 때문임

  spring.jpa.show-sql=true
  spring.jpa.properties.hibernate.format_sql=true
  - 실행되는 SQL을 콘솔에 출력하고 포맷팅해서 보여주는 설정임
  - 테스트 중 어떤 쿼리가 실행되는지 디버깅하기 위해 필요함

  spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
  - PostgreSQL 데이터베이스를 사용한다고 Hibernate에 알려주는 것임
  - DB마다 SQL 문법이 약간씩 다르기 때문에 명시해야 함

  3. Redis/Session 설정 (15-23번 줄)

  spring.data.redis.host=localhost
  spring.data.redis.port=${TEST_CONTAINER_REDIS_PORT:26381}
  - TestContainer로 띄운 Redis에 연결하기 위한 설정임
  - 포트는 환경변수로 주입받되, 없으면 기본값 26381을 사용함
  - TestContainer가 랜덤 포트를 할당하면 TEST_CONTAINER_REDIS_PORT로 주입할 수 있도록 한 것임

  spring.data.redis.password=${TEST_CONTAINER_REDIS_PASSWORD:PT_R3dis_T3stC0ntain3r!}
  - Redis 인증 비밀번호임
  - 마찬가지로 환경변수 주입 방식이며, 기본값은 테스트용 비밀번호임

  spring.data.redis.timeout=2000ms
  - Redis 연결 타임아웃을 2초로 설정한 것임
  - 테스트 환경에서는 빠른 실패를 위해 짧게 설정하는 게 일반적임

  spring.session.redis.flush-mode=on_save
  spring.session.redis.namespace=spring:session:test
  - Spring Session을 Redis에 저장하는 설정임
  - on_save: 세션 데이터가 변경되면 즉시 Redis에 저장함
  - namespace: Redis 키에 spring:session:test 접두사를 붙여서 다른 환경과 구분함

  server.servlet.session.timeout=30m
  - 세션 만료 시간을 30분으로 설정한 것임

  4. 서버 포트 설정 (25-27번 줄)

  server.port=0
  - 0번 포트: OS가 사용 가능한 랜덤 포트를 자동으로 할당하도록 하는 설정임
  - 테스트를 병렬로 실행할 때 포트 충돌을 방지하기 위해 사용함
  - 여러 테스트가 동시에 실행되어도 각자 다른 포트를 사용하게 되는 것임

  ---
  요약

  이 설정 파일의 핵심 목적:
  1. 격리된 테스트 환경: 프로덕션과 완전히 분리된 설정
  2. TestContainer 통합: Docker 컨테이너로 띄운 PostgreSQL/Redis와 연결
  3. 병렬 실행 지원: 랜덤 포트 할당으로 포트 충돌 방지
  4. 디버깅 편의성: SQL 로그 출력으로 쿼리 확인 가능

  이런 식으로 구성한 이유는 실제 DB/Redis를 사용하는 통합 테스트를 로컬에서 쉽게 실행하기 위함임.

```