---
tags:
  - Project/PulseTicket
---

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
