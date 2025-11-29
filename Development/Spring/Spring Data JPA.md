---
tags:
  - Spring/Data
  - Java/JPA
---

## JPA (Java Persistence API)

자바 ORM(Object-Relational Mapping) 기술의 표준 인터페이스.
RDBMS의 데이터를 객체지향적으로 관리할 수 있게 해줌.
구현체로 Hibernate가 가장 널리 사용됨.

### Persistence Context (영속성 컨텍스트)
- **Entity Manager**: 영속성 컨텍스트를 관리하는 객체.
- **Persistence**: "영속성" - 프로그램이 종료되어도 데이터가 사라지지 않는 속성.
- **Lifecycle**:
    - **Transient (비영속)**: 객체만 생성된 상태.
    - **Managed (영속)**: EntityManager에 의해 관리되는 상태 (persist).
    - **Detached (준영속)**: 영속성 컨텍스트에서 분리된 상태.
    - **Removed (삭제)**: 삭제된 상태.

## Spring Data JPA

`JpaRepository` 인터페이스를 상속받아 기본적인 CRUD 기능을 제공하는 Spring 모듈.
메서드 이름으로 쿼리를 자동 생성하는 기능을 제공.

### Query Methods

```java
// 메서드 이름으로 쿼리 생성
List<Seat> findBySeatNumber(String seatNumber);
List<Seat> findByIsAvailableTrue();
List<Seat> findByPriceBetween(BigDecimal min, BigDecimal max);
```

### @Query
복잡한 쿼리나 JPQL을 직접 작성할 때 사용.
```java
@Query("SELECT s FROM Seat s WHERE s.row = :row AND s.isAvailable = :available")
List<Seat> findByRowAndAvailable(@Param("row") String row, @Param("available") boolean available);
```

### Entity Mapping & Relationships

#### 1. 연관 관계 (@ManyToOne, @OneToMany)
- **@ManyToOne**: 다대일 관계 (예: 예약 N : 사용자 1). 외래키를 가진 쪽이 주인(Owner).
- **@OneToMany**: 일대다 관계. `mappedBy` 속성으로 주인이 아님을 명시 (읽기 전용).
- **@OneToOne**: 일대일 관계.
- **@ManyToMany**: 다대다 관계 (실무에서는 중간 테이블 엔티티로 풀어서 사용 권장).

#### 2. FetchType (Lazy vs Eager)
- **LAZY (지연 로딩)**: 실제로 데이터를 사용할 때 쿼리가 나감. (권장)
    - `N+1 문제` 발생 가능 -> `join fetch` 등으로 해결.
- **EAGER (즉시 로딩)**: 엔티티 조회 시 연관된 데이터도 한 번에 조인해서 가져옴. 예상치 못한 쿼리 발생 가능.

#### 3. Cascade
영속성 전이. 부모 엔티티의 상태 변화를 자식에게 전파.
- `CascadeType.ALL`, `PERSIST`, `REMOVE` 등.

#### 4. ID Generation Strategies (@GeneratedValue)
- **IDENTITY**: DB의 Auto Increment 사용 (MySQL). insert 시점에 ID를 알기 위해 즉시 쿼리 실행 (Batch Insert 불가).
- **SEQUENCE**: DB Sequence 사용 (Oracle, PG). 시퀀스를 미리 받아와서 Batch Insert 가능.
- **AUTO**: DB 방언에 따라 자동 선택.

### Entity Lifecycle Callbacks (@PrePersist, @PostPersist, ...)

엔티티의 생명주기 이벤트에 훅(Hook)을 걸어 로직 수행.

| 어노테이션 | 시점 | 용도 예시 |
| :--- | :--- | :--- |
| `@PrePersist` | DB 저장 전 | 생성일자(`createdAt`), 기본값 설정 |
| `@PostPersist` | DB 저장 후 | ID 생성 확인, 이벤트 발행 (이메일 발송 등) |
| `@PreUpdate` | 수정 전 | 수정일자(`updatedAt`) 갱신 |
| `@PostUpdate` | 수정 후 | 캐시 무효화, 변경 알림 |

**주의**: `@PostPersist` 등에서 다시 `save()`를 호출하면 무한 루프 위험. 비동기 이벤트 발행 등에 적합.

## JPA Best Practices

- **Setter 사용 지양**: 객체의 일관성을 위해 무분별한 Setter 대신 명확한 비즈니스 메서드(예: `changeAddress()`) 사용.
- **기본 생성자**: JPA 스펙상 `protected` 이상의 기본 생성자(`@NoArgsConstructor`) 필수.
- **N+1 문제 주의**: 지연 로딩 사용 시 반복문 내에서 연관 엔티티 조회하면 쿼리 폭발. `Fetch Join`이나 `@EntityGraph`로 해결.
