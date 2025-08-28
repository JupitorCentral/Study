- [ ] Backend Developer Technical Interview: WebSocket, Redis, and Spring ➕ 2025-08-27 

---

### ||Q||: Asking about the differences between WebSocket and HTTP

좋습니다. 백엔드 개발자 기술면접을 시작하겠습니다.

실시간 통신 관련 질문

이력서를 보니 WebSocket 기반 실시간 연동 시스템을 개발하신 경험이 있네요. 그럼 첫 번째 질문입니다:

**WebSocket과 일반 HTTP 통신의 차이점은 무엇이고, 언제 WebSocket을 사용해야 하는지 설명해보세요.**

참고로, 단순히 "실시간 통신이 가능하다" 정도의 답변보다는 프로토콜 레벨에서의 차이점과 구체적인 사용 시나리오를 중심으로 답변해주시면 됩니다.

### ||A||: Answering about WebSocket vs. HTTP

http 통신은 stateless, 즉 서버가 사용자의 세션을 저장하고 있지 않지만, websocket은 서버와의 연결을 유지합니다.

HTTP 통신의 경우 비동기화 적인 동작, 즉 일반적인 웹서핑 등에 쓰이지만 websocket 의 경우 방송 스트리밍, 또는 게임과 같은 다른 사용자의 액션에 대한 즉각적인 화면 반응이 그 쓰임새가 되겠습니다.

### ||Q||: Clarifying the interviewee's answer on WebSocket vs. HTTP and asking a follow-up question

음, 기본적인 개념은 이해하고 계시는 것 같은데 몇 가지 부정확한 부분이 있네요.

#### Expression Check

첫째, HTTP도 세션을 저장할 수 있습니다. 쿠키나 세션 스토리지를 통해서 말이죠. HTTP가 stateless라는 것은 각 요청이 독립적이라는 의미이지, 세션 저장이 불가능하다는 뜻은 아닙니다.

둘째, "비동기화적인 동작"이라고 하셨는데 HTTP도 AJAX를 통해 비동기 통신이 가능합니다.

**그럼 다시 질문드리겠습니다: WebSocket과 HTTP의 가장 근본적인 차이점은 무엇일까요? 프로토콜 레벨에서 생각해보세요.**

힌트를 드리자면, 통신 방향성과 연결 방식에 대해 생각해보시기 바랍니다.

### ||A||: Stating inability to answer

잘 모르겠습니다.

### ||Q||: Explaining the fundamental differences between WebSocket and HTTP and asking about Redis Pub/Sub

설명해드리겠습니다.

#### HTTP vs WebSocket의 근본적 차이점

1. **통신 방향성**
    
    - **HTTP**: 클라이언트 → 서버로만 요청 시작 가능 (Request-Response 패턴)
        
    - **WebSocket**: 양방향 통신 가능, 서버도 먼저 클라이언트에게 메시지 전송 가능
        
2. **연결 방식**
    
    - **HTTP**: 요청-응답 후 연결 종료 (Keep-Alive로 유지 가능하지만 본질적으로는 단발성)
        
    - **WebSocket**: 한 번 연결하면 지속적으로 연결 유지 (Persistent Connection)
        
3. **오버헤드**
    
    - **HTTP**: 매번 헤더 정보 전송 (쿠키, 인증 등)
        
    - **WebSocket**: 초기 handshake 후에는 최소한의 프레임 헤더만 전송
        

예를 들어, 채팅 앱에서 새 메시지가 올 때:

- **HTTP**: 클라이언트가 계속 "새 메시지 있나요?" 물어봐야 함 (Polling)
    
- **WebSocket**: 서버가 "새 메시지 왔어요!" 먼저 알려줄 수 있음
    

**이제 다음 질문으로 넘어가겠습니다. 당신이 개발한 온라인 공부방 프로젝트에서 Redis Pub/Sub을 사용하셨다고 했는데, 왜 Redis를 선택하셨나요? 그리고 Redis 없이 WebSocket만으로는 왜 안 되는 건가요?**

이 질문은 분산 시스템과 스케일링에 대한 이해도를 확인하는 질문입니다.

### ||A||: Stating inability to answer

잘 모르겠습니다. (영어권에서 검색할것. 근거는 무조건)

### ||Q||: Explaining performance issues with Redis Pub/Sub at scale and asking about DB Connection Pools

설명해드리겠습니다.

#### Redis Pub/Sub 대규모 서비스에서의 성능 이슈

1. **연결 수 제한**
    
    - Redis는 기본적으로 `maxclients` 설정으로 최대 10,000개의 동시 연결만 허용
        
    - 10만 명 동접자라면 연결 풀링이나 여러 Redis 인스턴스 분산이 필요
        
2. **메시지 지속성 문제**
    
    - Redis Pub/Sub은 메시지를 저장하지 않음
        
    - 구독자가 오프라인이면 메시지 손실 발생
        
    - 실시간성은 좋지만 신뢰성에 문제
        
3. **Redis Cluster에서의 확장성 문제**
    
    - 클러스터 환경에서 모든 노드에 메시지를 브로드캐스트
        
    - 노드가 10개, 메시지 1KB라면 처리량이 12.5K RPS로 제한
        
    - 노드가 늘어날수록 오히려 성능 저하 (negative scaling)
        
4. **네트워크 대역폭 제한**
    
    - 구독자 수 × 메시지 크기만큼 네트워크 사용량 증가
        
    - 대규모 서비스에서는 네트워크가 병목점이 됨
        

해결책으로는 채널 분산, Redis Streams 사용, 또는 Kafka와 같은 다른 메시지 브로커 고려가 있습니다.

**이제 다음 질문으로 넘어가겠습니다. Spring Boot에서 MySQL을 사용할 때, 데이터베이스 연결 풀(Connection Pool)을 설정하는 이유는 무엇이고, 어떤 설정값들을 주로 튜닝하시나요?**

이력서를 보니 MySQL 경험이 있으시니, 실무에서 중요한 DB 성능 최적화에 대해 질문드립니다.

### ||A||: Asking clarifying questions about Redis Pub/Sub issues

잠시만요. 이 질문에 대한 의문점이 있습니다.

1. 메시지 지속성 문제와 관련하여, 대용량 트래픽 환경에서 신뢰성이 중요한 이유는 무엇인가요? 단순히 데이터 양이 많아져서 Redis에서 데이터 손실 가능성이 커지기 때문인가요?
    
2. Redis Cluster의 확장성 문제가 'negative scaling'으로 이어진다고 하셨는데, 이것은 근본적으로 네트워크 사양의 한계 때문에 발생하는 문제인가요?
    

### ||Q||: Answering clarifying questions on Redis and asking about Spring Transaction Propagation

아주 좋은 질문입니다! 단순히 답변을 받아들이지 않고 깊이 생각해보시는 모습이 인상적이네요.

#### 1. 대용량 트래픽에서 왜 신뢰성이 필요한가?

양이 때문만이 아닙니다. 주요 이유들을 설명드리겠습니다:

- **비즈니스 크리티컬한 상황들**:
    
    - **금융 거래**: 주식 체결, 송금 알림 등에서 메시지 손실은 직접적인 금전적 피해
        
    - **전자상거래**: 주문 확정, 재고 변경 알림 손실 시 중복 주문이나 품절 상황 발생
        
    - **의료/안전 시스템**: 응급 알림 손실은 생명과 직결
        
- **사용자 경험 측면**:
    
    - 사용자가 잠시 네트워크가 끊어졌다가 돌아왔을 때, 중간에 놓친 메시지들을 받을 수 있어야 함
        
    - 채팅 앱에서 메시지가 사라지면 사용자 신뢰도 하락
        
- **대용량에서 더 중요해지는 이유**:
    
    - 사용자 수가 많을수록 메시지 손실의 **절대적 피해 규모가 커짐**
        
    - 10만 명 중 1%만 메시지를 놓쳐도 1,000명이 피해를 봄
        

#### 2. Redis Cluster 확장성 문제가 네트워크 사양 때문인가?

네트워크 사양도 일부 원인이지만, 더 근본적인 문제는 **아키텍처**입니다.

- **구조적 문제**:
    
    - Redis Pub/Sub은 클러스터의 **모든 노드가 모든 메시지를 받아야 하는 구조**
        
    - 10개 노드에서 1KB 메시지를 브로드캐스트하면 → 총 10KB의 네트워크 트래픽
        
    - 노드가 20개로 늘어나면 → 총 20KB로 증가 (선형 증가)
        
- **계산해보면**:
    
    - `처리량 = 네트워크 대역폭 ÷ (메시지 크기 × 노드 수)`
        
    - 예: `125MB/s ÷ (1KB × 10노드) = 12.5K RPS`
        
- **왜 negative scaling인가**:
    
    - 노드를 늘려서 처리 능력을 높이려 했지만
        
    - 네트워크 오버헤드가 더 크게 증가해서
        
    - 오히려 전체 처리량이 감소
        
- **해결책**:
    
    - **Message Sharding**: 채널을 노드별로 분산
        
    - **다른 브로커 사용**: Kafka, RabbitMQ 등은 이 문제를 다르게 해결
        
    - **Redis Streams**: Redis 5.0부터 제공되는 대안
        

이런 질문을 하신 것을 보니 단순 암기가 아닌 근본 원리를 이해하려는 자세가 보입니다. 면접에서는 이런 태도가 매우 중요하죠.

**그럼 다음 질문입니다: Spring Boot에서 트랜잭션 전파(Transaction Propagation)에 대해 설명해보세요. 특히 REQUIRES_NEW와 REQUIRED의 차이점은 무엇인가요?**

### ||A||: Stating inability to answer

전혀 모르겠네요. ㅠㅠ

### ||Q||: Explaining Spring Transaction Propagation and asking about DI annotations

설명해드리겠습니다.

#### 트랜잭션 전파(Transaction Propagation)란?

트랜잭션 전파는 이미 트랜잭션이 시작된 상태에서 다른 트랜잭션이 필요한 메서드가 호출될 때, 어떻게 처리할 것인지를 정하는 설정입니다.

Spring에서는 `@Transactional` 애노테이션의 `propagation` 속성으로 설정할 수 있습니다.

#### REQUIRED vs REQUIRES_NEW 차이점

- **PROPAGATION_REQUIRED** (기본값)
    
    Java
    
    ```
    @Transactional(propagation = Propagation.REQUIRED)
    public void methodA() {
        // 비즈니스 로직
        methodB(); // REQUIRED로 설정된 메서드 호출
    }
    
    @Transactional(propagation = Propagation.REQUIRED)
    public void methodB() {
        // 비즈니스 로직
    }
    ```
    
    - **기존 트랜잭션이 있으면**: 그 트랜잭션에 참여
        
    - **기존 트랜잭션이 없으면**: 새로운 트랜잭션 시작
        
    - **결과**: `methodA`와 `methodB`가 **같은 트랜잭션**에서 실행
        
- **PROPAGATION_REQUIRES_NEW**
    
    Java
    
    ```
    @Transactional(propagation = Propagation.REQUIRED)
    public void methodA() {
        // 비즈니스 로직
        methodC(); // REQUIRES_NEW로 설정된 메서드 호출
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void methodC() {
        // 비즈니스 로직
    }
    ```
    
    - **항상 새로운 트랜잭션**을 시작
        
    - **기존 트랜잭션이 있으면**: 기존 트랜잭션을 일시 중단(suspend)하고 새 트랜잭션 시작
        
    - **결과**: `methodA`와 `methodC`가 **별도의 독립적인 트랜잭션**에서 실행
        

#### 실제 동작 차이점

- **REQUIRED 예시**
    
    Java
    
    ```
    @Transactional
    public void outerMethod() {
        // 트랜잭션 A 시작
        updateUserInfo();
        updateUserPoints(); // 같은 트랜잭션 A에서 실행
        // 만약 updateUserPoints()에서 예외 발생하면
        // updateUserInfo()도 함께 롤백됨
    }
    ```
    
- **REQUIRES_NEW 예시**
    
    Java
    
    ```
    @Transactional
    public void outerMethod() {
        // 트랜잭션 A 시작
        updateUserInfo();
        logUserActivity(); // 새로운 트랜잭션 B에서 실행
        // logUserActivity()에서 예외가 발생해도
        // updateUserInfo()는 롤백되지 않음 (별도 트랜잭션이므로)
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logUserActivity() {
        // 독립적인 트랜잭션에서 실행
    }
    ```
    

#### 언제 사용하나?

- **REQUIRED**:
    
    - 일반적인 상황 (기본값)
        
    - 모든 작업이 하나의 논리적 단위로 처리되어야 할 때
        
- **REQUIRES_NEW**:
    
    - 로깅이나 감사(Audit) 작업처럼 메인 비즈니스 로직과 독립적으로 처리되어야 할 때
        
    - 부분적 실패가 전체 트랜잭션에 영향을 주지 않아야 할 때
        

**이제 다음 질문입니다. Spring에서 `@Autowired`, `@Resource`, `@Inject`의 차이점은 무엇인가요? 특히 어떤 기준으로 의존성을 주입하는지 설명해보세요.**

### ||A||: Stating inability to answer

잘 모르겠습니다.

### ||Q||: Explaining the differences between @Autowired, @Resource, and @Inject

Spring에서 `@Autowired`, `@Resource`, `@Inject`의 차이점에 대해 설명해드리겠습니다.

#### 각 애노테이션의 기본 정보

1. **@Autowired (Spring 제공)**
    
    - Spring Framework에서 제공하는 애노테이션
        
    - **타입 기반(byType)** 의존성 주입이 우선
        
    - Spring 2.5부터 사용 가능
        
2. **@Resource (JSR-250 표준)**
    
    - Java 표준 애노테이션 (JSR-250)
        
    - **이름 기반(byName)** 의존성 주입이 우선
        
    - `name` 속성을 통해 구체적인 빈 이름 지정 가능
        
3. **@Inject (JSR-330 표준)**
    
    - Java 표준 애노테이션 (JSR-299/JSR-330)
        
    - **타입 기반(byType)** 의존성 주입
        
    - `@Autowired`와 동일한 동작
        

#### 의존성 주입 방식의 차이

- **@Autowired - 타입 우선**
    
    Java
    
    ```
    @Service
    public class UserService {
        @Autowired
        private UserRepository userRepository; // 타입으로 찾음
    }
    ```
    
- **@Resource - 이름 우선**
    
    Java
    
    ```
    @Service
    public class UserService {
        @Resource(name = "userRepository") // 이름을 명시적 지정
        private UserRepository userRepository;
    
        @Resource // 필드명(userRepository)으로 찾음
        private UserRepository userRepository;
    }
    ```
    
- **@Inject - 타입 우선**
    
    Java
    
    ```
    @Service
    public class UserService {
        @Inject
        private UserRepository userRepository; // @Autowired와 동일
    }
    ```
    

#### 실제 동작 차이점

##### 같은 타입의 빈이 여러 개 있는 경우

Java

```
@Repository("userRepo1")
public class UserRepositoryImpl1 implements UserRepository { }

@Repository("userRepo2")
public class UserRepositoryImpl2 implements UserRepository { }
```

- **@Autowired 사용시**:
    
    Java
    
    ```
    @Autowired
    private UserRepository userRepository; // 에러! 어떤 빈을 주입할지 모름
    
    @Autowired
    @Qualifier("userRepo1") // Qualifier로 구체적 지정 필요
    private UserRepository userRepository;
    ```
    
- **@Resource 사용시**:
    
    Java
    
    ```
    @Resource(name = "userRepo1") // name으로 직접 지정
    private UserRepository userRepository; // 정상 동작
    ```
    

#### 언제 어떤 것을 사용할까?

- **@Autowired 추천 상황**
    
    - 일반적인 Spring 애플리케이션
        
    - 타입이 명확하고 같은 타입의 빈이 하나뿐인 경우
        
- **@Resource 추천 상황**
    
    - 같은 타입의 빈이 여러 개 있어 이름으로 구분이 필요한 경우
        
    - Java EE 환경과의 호환성이 중요한 경우
        
- **@Inject 추천 상황**
    
    - JSR-330 표준을 준수하고 싶은 경우
        
    - Spring 외의 DI 컨테이너 사용 가능성이 있는 경우
        

핵심은 **@Autowired와 @Inject는 타입 기반**, **@Resource는 이름 기반 주입**이라는 점입니다!

### ||A||: Commenting on the benefits of Dependency Injection and asking for confirmation

의존성 주입(loosely coupled)을 사용하면 코드의 재사용성 측면에서 효율성이 매우 높아지는군요. 만약 의존성 주입을 사용하지 않는다면, 클래스 인스턴스가 생성될 때마다 의존하는 객체를 코드상에서 직접 새로 생성해주어야 하는 것인가요? 아니면 단순히 클래스의 멤버 변수로 정의하는 것과는 큰 차이가 없는 것인지 궁금합니다.

### ||Q||: Confirming and elaborating on the benefits of Dependency Injection

좋은 관찰입니다! 의존성 주입(Dependency Injection)의 가치에 대해 정확히 핵심을 짚으셨네요. 하지만 좀 더 명확하게 정리해서 설명해드리겠습니다.

#### 의존성 주입 vs 직접 객체 생성의 차이

1. **직접 객체 생성 방식의 문제점 (Tightly Coupled)**
    
    Java
    
    ```
    public class OrderService {
        private EmailService emailService;
        private PaymentService paymentService;
    
        public OrderService() {
            this.emailService = new EmailService(); // 직접 생성
            this.paymentService = new PaymentService(); // 직접 생성
        }
    }
    ```
    
    - **테스트 어려움**: Mock 객체로 바꿀 수 없어 테스트가 힘듦
        
    - **유연성 부족**: `EmailService`를 `SMS`로 바꾸려면 코드 수정 필요
        
    - **재사용성 저하**: `OrderService`는 특정 구현체에 강하게 결합됨
        
2. **의존성 주입 방식의 장점 (Loosely Coupled)**
    
    Java
    
    ```
    @Service
    public class OrderService {
        private final EmailService emailService;
        private final PaymentService paymentService;
    
        // Spring이 자동으로 주입
        @Autowired
        public OrderService(EmailService emailService, PaymentService paymentService) {
            this.emailService = emailService;
            this.paymentService = paymentService;
        }
    }
    ```
    

#### 실제 차이점을 보여주는 예시: 알림 서비스 변경

- **직접 생성 방식**
    
    Java
    
    ```
    // SMS로 바꾸려면 코드 수정 필요
    public class OrderService {
        private SMSNotification notification = new SMSNotification(); // 코드 수정!
    
        public void processOrder() {
            notification.send("주문 완료");
        }
    }
    ```
    
- **의존성 주입 방식**
    
    Java
    
    ```
    public interface NotificationService {
        void send(String message);
    }
    
    @Service
    public class OrderService {
        private final NotificationService notification;
    
        @Autowired // Spring이 적절한 구현체를 주입
        public OrderService(NotificationService notification) {
            this.notification = notification;
        }
    
        public void processOrder() {
            notification.send("주문 완료"); // 코드 수정 불필요!
        }
    }
    ```
    

#### 테스트에서의 차이

- **직접 생성 방식**
    
    Java
    
    ```
    @Test
    public void testOrderService() {
        OrderService service = new OrderService();
        // EmailService가 실제로 이메일을 보내버림!
        // 테스트가 외부 의존성에 영향받음
        service.processOrder();
    }
    ```
    
- **의존성 주입 방식**
    
    Java
    
    ```
    @Test
    public void testOrderService() {
        // Mock 객체 생성
        NotificationService mockNotification = mock(NotificationService.class);
        OrderService service = new OrderService(mockNotification);
    
        // 실제 이메일/SMS 안 보내고 테스트 가능
        service.processOrder();
        verify(mockNotification).send("주문 완료");
    }
    ```
    

#### 핵심 장점 정리

1. **코드 재사용성** ✓
    
    - 같은 `OrderService`를 다른 `NotificationService`와 조합 가능
        
2. **유연성** ✓
    
    - 런타임에 다른 구현체로 교체 가능
        
3. **테스트 용이성** ✓
    
    - Mock 객체로 쉽게 단위 테스트 가능
        
4. **관심사 분리** ✓
    
    - `OrderService`는 비즈니스 로직에만 집중
        
    - 객체 생성/관리는 Spring Container가 담당
        

결론적으로, 단순히 "클래스로 정의"하는 것과는 차원이 다른 이점들이 있습니다. 의존성 주입은 코드의 유연성, 테스트 가능성, 재사용성을 획기적으로 개선해주는 핵심적인 설계 패턴입니다.

---

### Questions

1. WebSocket과 HTTP의 근본적인 차이점을 프로토콜 레벨(통신 방향성, 연결 방식)에서 설명해 보세요.
    
2. 분산 시스템에서 여러 서버 인스턴스가 실시간 통신을 해야 할 때, WebSocket만 사용하는 것이 왜 불충분하며 Redis Pub/Sub과 같은 메시지 브로커는 어떤 역할을 하나요?
    
3. 대규모 트래픽 환경에서 Redis Pub/Sub을 사용할 때 발생할 수 있는 성능 및 신뢰성 이슈는 무엇인가요?
    
4. 데이터베이스 커넥션 풀(Connection Pool)을 사용하는 이유는 무엇인가요?
    
5. Spring의 트랜잭션 전파(Transaction Propagation)에 대해 설명하고, `REQUIRED`와 `REQUIRES_NEW`의 핵심적인 차이점과 각각의 사용 사례를 들어보세요.
    
6. Spring의 의존성 주입 애노테이션 `@Autowired`, `@Resource`, `@Inject`를 비교 설명하고, 각각 어떤 기준으로 주입할 빈(Bean)을 결정하는지 설명해 보세요.
    
7. 의존성 주입(DI)을 사용함으로써 얻는 핵심적인 이점들을 테스트 용이성, 유연성, 재사용성 측면에서 설명해 보세요.