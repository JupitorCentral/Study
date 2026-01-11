---
tags:
  - Project/PulseTicket
---

# 레디스 캐싱 적용

## **1단계: 환경 설정**

- [x] Docker로 Redis 컨테이너 띄우기 (포트 6379) ✅ 2026-01-11
    
- [x] Spring Boot 프로젝트에 `spring-boot-starter-data-redis` 의존성 추가하기 ✅ 2026-01-11
    
- [x] `application.yml`에 Redis 연결 설정(host, port) 추가하기 ✅ 2026-01-11
    

## **2단계: Redis 연동 코드 작성**

- [x] `RedisTemplate` 또는 `StringRedisTemplate` 설정(Configuration) 클래스 작성하기 ✅ 2026-01-11
    
- [x] Redis 서비스 클래스 만들기 (`ReservationRedisService`) ✅ 2026-01-11
    
- [ ] Redis 서비스에 **좌석 선점 메서드** 구현하기
    
- [ ]  Redis 서비스에 **좌석 선점 해제(삭제) 메서드** 구현하기

## **3단계: 비즈니스 로직 연결**

- [ ]  예약 요청 API (`POST /reservations`)에서 Redis 좌석 선점 메서드 호출하기
    
    - [ ] 성공 시: "예약 대기 성공" 응답
        
    - [ ] 실패 시: "이미 선택된 좌석" 예외 처리
        
-  결제 완료 API (`POST /payments`)에서:
    
    1. [ ] DB에 예매 정보 `INSERT` (최종 저장)    
    2. [ ] Redis 좌석 선점 해제 메서드 호출 (키 삭제)
        

## **4단계: 테스트 및 검증**

- [ ]  Postman으로 같은 좌석에 대해 동시에 2번 요청 보내서, 1명만 성공하는지 확인하기 (동시성 테스트)
    
- [ ]  Redis CLI(`redis-cli`) 접속해서 `TTL` 명령어로 만료 시간이 줄어드는지 눈으로 확인하기
    
- [ ]  결제 완료 후 Redis 키가 사라지는지 확인하기






## PulseTicket Project Roadmap

### 1. ID Generation Strategy
- `@GeneratedValue` 전략 선택 (IDENTITY vs SEQUENCE).
- MySQL(Identity) vs PostgreSQL(Sequence) 차이 이해.

### 2. N+1 Problem
- **원인**: JPA 조회 시 연관된 엔티티를 가져오기 위해 추가 쿼리가 N번 발생하는 문제.
- **해결**: `Fetch Join` (JPQL `join fetch`), `@EntityGraph`, `Batch Size` 설정.

### 3. Pagination & Sorting
- 대량 데이터 조회 최적화.
- `Pageable` 인터페이스 활용.

### 4. Auditing
- 생성일(`createdAt`), 수정일(`updatedAt`) 자동 관리.
- `@EnableJpaAuditing`, `@CreatedDate`, `@LastModifiedDate`.

### 5. Concurrency Control (Optimistic Locking)
- **낙관적 락**: DB 락 대신 버전 관리(`@Version`)로 충돌 감지.
- 티켓 예매 시 중복 예약 방지에 필수.


# 고민해야 할 것

## **Q1. "선점 메서드(락 획득)가 실패했을 때, 사용자에게 바로 에러를 뱉을까요, 아니면 0.1초 뒤에 다시 시도하게 할까요?"**

- (힌트: **Spin Lock** vs **Pub/Sub**. 그냥 튕겨내면 유저 경험이 안 좋고, 계속 재시도하면 Redis가 힘들어합니다.)
    

## **Q2. "서버가 락을 걸고 나서(3번), 락을 해제(4번)하기도 전에 서버가 셧다운되면 그 좌석은 어떻게 되나요?"**

- (힌트: **TTL(Time To Live)** 설정. 락에도 수명을 줘야 합니다.)
    

## **Q3. "RedisTemplate의 Serializer를 설정할 때 `GenericJackson2JsonRedisSerializer`와 `StringRedisSerializer` 중 무엇이 락 구현에 더 적합할까요?"**

- (힌트: 락의 Key는 문자열로 명확해야 하므로 Key에는 `StringRedisSerializer`가 필수입니다.)


### 1. 왜 `StringRedisSerializer`인가?

락을 구현할 때 Redis에 저장하는 값(Value)이 무엇인지 생각해보세요. 
보통은 "누가 락을 잡았는지" 식별하기 위한 **단순한 문자열(UUID 또는 Thread ID)** 일 뿐입니다.

- **가벼움과 직관성**: `StringRedisSerializer`는 자바의 문자열을 Redis의 바이트 배열로 그대로 변환합니다. 
  군더더기가 전혀 없습니다.
    
- **Lua 스크립트 호환성 (핵심)**: 분산 락의 정합성을 위해 락을 해제할 때 
  검증과 삭제를 원자적(Atomic)으로 처리하려고 **Lua 스크립트**를 반드시 사용하게 됩니다.
    
    - 이때 Lua 스크립트는 `ARGV[1]`로 들어온 요청 ID와 Redis에 저장된 값을 비교(` == `)합니다.
        
    - `StringRedisSerializer`를 쓰면 Redis에 저장된 값이 `"f8c3de3d-..."` 처럼 단순 문자열이므로 스크립트에서 비교가 매우 쉽습니다.
        

### 2. 왜 `GenericJackson2JsonRedisSerializer`는 부적합한가?

이 시리얼라이저는 객체의 클래스 타입 정보를 JSON에 포함시킵니다(`@class` 필드 등). 락 값으로 단순 문자열 `"my-lock-id"`를 저장하더라도, 실제 Redis에는 다음과 같이 저장될 가능성이 큽니다.

```json
{
  "@class": "java.lang.String",
  "string": "my-lock-id"
}
```

- **불필요한 오버헤드**: 고작 ID 하나 저장하는데 배보다 배꼽이 더 큽니다.
    
- **Lua 스크립트 복잡도 증가**: 스크립트 내에서 이 JSON을 파싱하거나, 
  아니면 클라이언트가 이 복잡한 JSON 포맷을 정확히 맞춰서 비교 요청을 보내야 합니다. 
  실수할 여지가 다분합니다.


> "시리얼라이저 종류를 묻기 전에, **락의 해제(Release) 과정에서 원자성(Atomicity)을 어떻게 보장할 것인가?** 를 먼저 고민했어야 합니다."


