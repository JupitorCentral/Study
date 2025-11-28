
## JPA 순환 참조 문제

```java
@GetMapping("inquiryUserReservations")  
public List<Reservation> inquiryUserReservations(@ModelAttribute ReservationBookingRequest request) {  
    return reservationQueryService.inquiryUserReservations(request);  
```

```java
public List<Reservation> inquiryUserReservations(ReservationBookingRequest request) {  
    return reservationRepository.findByUser_LoginId(request.getLoginId());  
}
```

```java
public interface ReservationRepository extends JpaRepository<Reservation, Long> {  
    List<Reservation> findByUser_LoginId(String loginId);  
}
```

```java
/**  
 * 예약 정보를 관리하는 엔티티  
 */  
public class Reservation extends BaseEntity {  
...

    /**  
     * 예약한 사용자  
     */  
    @ManyToOne(fetch = FetchType.LAZY)  
    @JoinColumn(name = "user_id", nullable = false)  
    private User user;  
  
    ...
```

```java
public class User extends BaseEntity {  
  
	  ...
  
    /**  
     * 사용자의 예약 목록  
     */  
    @Builder.Default  
    @OneToMany(mappedBy = "user")  
    private List<Reservation> reservations = new ArrayList<>();  
}
```

