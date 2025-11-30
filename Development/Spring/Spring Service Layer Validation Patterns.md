---
tags:
  - Spring/Service
  - Java/Validation
---

## Service Level 에서 FK 관계를 validation 하는 방법

### 1. Validation Groups (가장 깔끔함)
^29a171

```java
// Generic FK Validator
@Component
public class ForeignKeyValidator {
    
    public <T, ID> void validateExists(
        CrudRepository<T, ID> repository,
        ID id,
        String fieldName
    ) {
        if (id != null && !repository.existsById(id)) {
            throw new InvalidForeignKeyException(fieldName, id);
        }
    }
}

// Service Layer
@Service
@RequiredArgsConstructor
public class SeatService {
    
    private final SeatRepository seatRepository;
    private final EventRepository eventRepository;
    private final ForeignKeyValidator fkValidator;
    
    /**
     * FK 검증 없이 좌석 생성 (논리상 이미 Event 존재가 확실한 경우)
     * 예: Event 생성 시 Seats를 함께 생성하는 경우
     */
    @Transactional
    public Seat createWithoutValidation(Seat seat) {
        return seatRepository.save(seat);
    }
    
    /**
     * FK 검증 포함하여 좌석 생성 (외부 요청으로 단독 생성하는 경우)
     * 예: 관리자가 특정 Event에 좌석을 추가하는 경우
     */
    @Transactional
    public Seat createWithValidation(Seat seat) {
        // Event 존재 여부 검증
        fkValidator.validateExists(
            eventRepository, 
            seat.getEventId(), 
            "event_id"
        );
        
        return seatRepository.save(seat);
    }
    
    /**
     * 여러 좌석을 한 번에 생성 (FK 검증은 한 번만)
     */
    @Transactional
    public List<Seat> createBulk(Long eventId, List<Seat> seats) {
        // Event 존재 여부를 한 번만 검증
        fkValidator.validateExists(eventRepository, eventId, "event_id");
        
        seats.forEach(seat -> {
            seat.setEventId(eventId);
            seat.prePersist();
        });
        
        return seatRepository.saveAll(seats);
    }
    
    /**
     * 좌석 상태 변경 (FK 검증 불필요 - 이미 존재하는 Seat)
     */
    @Transactional
    public Seat proceedStatus(Long seatId) {
        Seat seat = seatRepository.findById(seatId)
            .orElseThrow(() -> new SeatNotFoundException(seatId));
        
        seat.proceed();  // AVAILABLE -> RESERVED -> SOLD
        return seatRepository.save(seat);
    }
}
```

### 2. Custom Annotation으로 선택적 검증 - AOP

```java
// Custom Annotation
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidateForeignKeys {
    /**
     * 검증할 FK 필드 이름들
     */
    String[] fields();
}

// AOP Aspect for FK Validation
@Aspect
@Component
@RequiredArgsConstructor
public class ForeignKeyValidationAspect {
    
    private final ApplicationContext context;
    private final EventRepository eventRepository;
    // ... 다른 repositories
    
    @Before("@annotation(validateForeignKeys)")
    public void validateForeignKeys(
        JoinPoint joinPoint, 
        ValidateForeignKeys validateForeignKeys
    ) {
        Object[] args = joinPoint.getArgs();
        
        for (Object arg : args) {
            if (arg instanceof Seat seat) {
                for (String field : validateForeignKeys.fields()) {
                    validateField(seat, field);
                }
            }
        }
    }
    
    private void validateField(Seat seat, String field) {
        switch (field) {
            case "eventId" -> {
                if (!eventRepository.existsById(seat.getEventId())) {
                    throw new InvalidForeignKeyException("event_id", seat.getEventId());
                }
            }
            // 다른 FK 필드들...
        }
    }
}

// Service에서 사용
@Service
@RequiredArgsConstructor
public class SeatService {
    
    private final SeatRepository seatRepository;
    
    /**
     * FK 검증 없음
     */
    @Transactional
    public Seat create(Seat seat) {
        return seatRepository.save(seat);
    }
    
    /**
     * FK 검증 포함 - Annotation으로 선언적 표현
     */
    @Transactional
    @ValidateForeignKeys(fields = {"eventId"})
    public Seat createWithValidation(Seat seat) {
        return seatRepository.save(seat);
    }
}
```

### 3. Builder Pattern + Validation Strategy (가장 유연함)

```java
// Validation Strategy Interface
public interface SeatValidationStrategy {
    void validate(Seat seat);
}

// No Validation Strategy
@Component("noValidation")
public class NoValidationStrategy implements SeatValidationStrategy {
    @Override
    public void validate(Seat seat) {
        // Do nothing - 논리상 당연한 경우
    }
}

// Full Validation Strategy
@Component("fullValidation")
@RequiredArgsConstructor
public class FullValidationStrategy implements SeatValidationStrategy {
    
    private final EventRepository eventRepository;
    
    @Override
    public void validate(Seat seat) {
        if (!eventRepository.existsById(seat.getEventId())) {
            throw new InvalidForeignKeyException("event_id", seat.getEventId());
        }
    }
}

// Service
@Service
@RequiredArgsConstructor
public class SeatService {
    
    private final SeatRepository seatRepository;
    private final Map<String, SeatValidationStrategy> validationStrategies;
    
    /**
     * Validation strategy를 선택할 수 있는 범용 메서드
     */
    @Transactional
    public Seat create(Seat seat, String validationStrategy) {
        SeatValidationStrategy strategy = validationStrategies.get(validationStrategy);
        
        if (strategy == null) {
            throw new IllegalArgumentException("Unknown validation strategy: " + validationStrategy);
        }
        
        strategy.validate(seat);
        seat.prePersist();
        
        return seatRepository.save(seat);
    }
    
    // 편의 메서드들
    @Transactional
    public Seat createWithoutValidation(Seat seat) {
        return create(seat, "noValidation");
    }
    
    @Transactional
    public Seat createWithValidation(Seat seat) {
        return create(seat, "fullValidation");
    }
}
```
