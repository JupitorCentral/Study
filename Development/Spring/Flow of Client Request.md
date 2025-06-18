- [ ] Flow of Client Request ➕ 2025-06-17 

1. 클라이언트 Request
    ↓
2. Tomcat (Servlet Container)
    ↓
3. Spring Security Filter Chain
    ↓
4. DispatcherServlet (Front Controller)
    ↓
5. HandlerMapping (URL → Controller 매핑)
    ↓
6. HandlerAdapter (파라미터 바인딩)
    ↓
7. Controller (@RequestMapping)
    ↓
8. Service (@Service, 비즈니스 로직)
    ↓
9. Mapper (@Mapper, 데이터 접근)
    ↓
10. Database (Oracle)
    ↓
11. ViewResolver (뷰 이름 → 템플릿 파일)
    ↓
12. View (Thymeleaf 템플릿)
    ↓
13. HTTP Response (렌더링된 HTML)



## 1. 클라이언트 Request

**역할**: 웹 애플리케이션에 대한 모든 요청의 시작점

클라이언트(브라우저, 모바일 앱, API 클라이언트 등)가 HTTP 요청을 생성합니다. 이 요청에는 다음과 같은 정보가 포함됩니다:

- **HTTP 메서드** (GET, POST, PUT, DELETE 등)
    
- **URL 경로** (`/users/123`, `/api/products` 등)
    
- **헤더 정보** (Content-Type, Authorization, Accept 등)
    
- **요청 본문** (POST/PUT 요청의 경우)
    
- **쿼리 파라미터** (`?page=1&size=10`)
    

## 2. Tomcat (Servlet Container)

**역할**: Java 웹 애플리케이션을 실행하는 서블릿 컨테이너

Apache Tomcat은 Java EE 웹 애플리케이션 서버로, 다음과 같은 핵심 기능을 수행합니다:

- **HTTP 요청 수신**: 클라이언트로부터 오는 HTTP 요청을 받아들입니다
    
- **스레드 관리**: 각 요청을 별도의 스레드에서 처리합니다
    
- **서블릿 생명주기 관리**: 서블릿의 생성, 초기화, 실행, 소멸을 관리합니다
    
- **세션 관리**: HTTP 세션을 생성하고 관리합니다
    
- **정적 리소스 처리**: CSS, JavaScript, 이미지 파일 등을 직접 서빙합니다
    

Spring Boot에서는 내장 Tomcat이 기본으로 포함되어 있어 별도의 설치나 설정 없이 바로 사용할 수 있습니다.

## 3. Spring Security Filter Chain

**역할**: 보안 관련 처리를 담당하는 필터 체인

Spring Security는 서블릿 필터를 기반으로 동작하며, 요청이 애플리케이션 로직에 도달하기 전에 보안 검사를 수행합니다:

### 주요 필터들:

- **SecurityContextPersistenceFilter**: 보안 컨텍스트를 HTTP 세션에서 로드하고 저장
    
- **UsernamePasswordAuthenticationFilter**: 로그인 폼 기반 인증 처리
    
- **BasicAuthenticationFilter**: HTTP Basic 인증 처리
    
- **JwtAuthenticationFilter**: JWT 토큰 기반 인증 처리 (커스텀)
    
- **AuthorizationFilter**: 권한 검사 및 접근 제어
    
- **ExceptionTranslationFilter**: 보안 예외 처리
    

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(form -> form.loginPage("/login"))
            .build();
    }
}

```
## 4. DispatcherServlet (Front Controller)

**역할**: Spring MVC의 중앙 제어 지점

모든 HTTP 요청을 받아 적절한 컨트롤러로 라우팅하는 프론트 컨트롤러 패턴을 구현합니다:

- **요청 수신**: 모든 HTTP 요청을 중앙에서 받아들입니다
    
- **핸들러 매핑**: 요청 URL에 맞는 컨트롤러를 찾습니다
    
- **예외 처리**: 전역 예외 처리를 담당합니다
    
- **응답 생성**: 최종 응답을 클라이언트에게 전달합니다
    

Spring Boot에서는 자동 설정에 의해 `/*` 패턴으로 매핑되어 모든 요청을 처리합니다.

## 5. HandlerMapping (URL → Controller 매핑)

**역할**: 요청 URL을 적절한 컨트롤러 메서드와 매핑

요청된 URL과 HTTP 메서드를 분석하여 처리할 컨트롤러를 결정합니다:

### 주요 매핑 방식:

- **@RequestMapping**: 기본 매핑 어노테이션
    
- **@GetMapping**: GET 요청 전용 매핑
    
- **@PostMapping**: POST 요청 전용 매핑
    
- **@PathVariable**: URL 경로 변수 매핑
    
- **@RequestParam**: 쿼리 파라미터 매핑
    

```java

@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")  // GET /api/users/123
    public User getUser(@PathVariable Long id) {
        // 핸들러 매핑에 의해 이 메서드가 선택됨
    }
}

```


## 6. HandlerAdapter (파라미터 바인딩)

**역할**: 컨트롤러 메서드 실행 및 파라미터 바인딩

선택된 컨트롤러 메서드를 실제로 호출하고, HTTP 요청 데이터를 메서드 파라미터로 변환합니다:

### 주요 기능:

- **파라미터 바인딩**: HTTP 요청 데이터를 Java 객체로 변환
    
- **데이터 검증**: `@Valid` 어노테이션을 통한 입력 데이터 검증
    
- **타입 변환**: 문자열을 적절한 Java 타입으로 변환
    
- **메서드 호출**: 실제 컨트롤러 메서드를 호출


```java
@PostMapping("/users")
public ResponseEntity<User> createUser(
    @Valid @RequestBody UserCreateRequest request,  // JSON → Java 객체 변환
    @RequestHeader("Authorization") String token,   // 헤더 바인딩
    @RequestParam(defaultValue = "false") boolean sendEmail  // 쿼리 파라미터 바인딩
) {
    // HandlerAdapter가 모든 파라미터를 적절히 바인딩
}
```



## 7. Controller (@RequestMapping)

**역할**: HTTP 요청을 받아 비즈니스 로직을 호출하는 웹 계층

컨트롤러는 웹 요청을 받아 적절한 서비스를 호출하고 응답을 생성합니다:

### 주요 책임:

- **요청 검증**: 입력 데이터의 유효성 검사
    
- **서비스 호출**: 비즈니스 로직을 담당하는 서비스 계층 호출
    
- **응답 생성**: 처리 결과를 HTTP 응답으로 변환
    
- **예외 처리**: 컨트롤러 레벨의 예외 처리
    

```java
@RestController
@RequestMapping("/api/users")
@Validated
public class UserController {
    
    private final UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getUser(@PathVariable @Positive Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(UserResponse.from(user));
    }
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException e) {
        return ResponseEntity.notFound().build();
    }
}

```



## 8. Service (@Service, 비즈니스 로직)

**역할**: 애플리케이션의 핵심 비즈니스 로직을 처리하는 서비스 계층

서비스 계층은 실제 비즈니스 규칙과 로직을 구현합니다:

### 주요 책임:

- **비즈니스 로직 구현**: 도메인 규칙과 정책 적용
    
- **트랜잭션 관리**: `@Transactional`을 통한 데이터 일관성 보장
    
- **데이터 가공**: 여러 데이터 소스의 정보를 조합하고 가공
    
- **외부 서비스 연동**: 다른 시스템과의 통신
    

```java
@Service
@Transactional(readOnly = true)
public class UserService {
    
    private final UserMapper userMapper;
    private final EmailService emailService;
    
    @Transactional
    public User createUser(UserCreateRequest request) {
        // 비즈니스 규칙 검증
        validateUserCreation(request);
        
        // 사용자 생성
        User user = User.builder()
            .email(request.getEmail())
            .name(request.getName())
            .status(UserStatus.ACTIVE)
            .createdAt(LocalDateTime.now())
            .build();
            
        userMapper.insertUser(user);
        
        // 환영 이메일 발송 (비즈니스 로직)
        emailService.sendWelcomeEmail(user);
        
        return user;
    }
    
    private void validateUserCreation(UserCreateRequest request) {
        if (userMapper.existsByEmail(request.getEmail())) {
            throw new DuplicateEmailException("이미 존재하는 이메일입니다.");
        }
    }
}

```



## 9. Mapper (@Mapper, 데이터 접근)

**역할**: 데이터베이스와의 상호작용을 담당하는 데이터 접근 계층

MyBatis의 `@Mapper` 인터페이스를 통해 SQL 쿼리를 실행하고 결과를 Java 객체로 매핑합니다:

### 주요 기능:

- **SQL 매핑**: XML 또는 어노테이션을 통한 SQL 쿼리 정의
    
- **결과 매핑**: 데이터베이스 결과를 Java 객체로 자동 변환
    
- **파라미터 바인딩**: Java 객체를 SQL 파라미터로 변환
    
- **동적 쿼리**: 조건에 따른 동적 SQL 생성
    

```java
@Mapper
public interface UserMapper {
    
    @Select("SELECT * FROM users WHERE id = #{id}")
    Optional<User> findById(@Param("id") Long id);
    
    @Insert("INSERT INTO users (email, name, status, created_at) " +
            "VALUES (#{email}, #{name}, #{status}, #{createdAt})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    void insertUser(User user);
    
    @Update("UPDATE users SET name = #{name}, updated_at = NOW() WHERE id = #{id}")
    int updateUser(User user);
    
    // 동적 쿼리 예시 (XML 매핑 파일 사용)
    List<User> findUsers(UserSearchCondition condition);
}

```


## 10. Database (Oracle)

**역할**: 애플리케이션 데이터의 영구 저장소

Oracle 데이터베이스는 엔터프라이즈급 관계형 데이터베이스로 다음과 같은 기능을 제공합니다:

### 주요 특징:

- **ACID 트랜잭션**: 데이터 일관성과 무결성 보장
    
- **동시성 제어**: 여러 사용자의 동시 접근 관리
    
- **백업 및 복구**: 데이터 손실 방지
    
- **성능 최적화**: 인덱스, 파티셔닝 등을 통한 쿼리 성능 향상
    


```sql
-- 사용자 테이블 예시
CREATE TABLE users (
    id NUMBER PRIMARY KEY,
    email VARCHAR2(255) UNIQUE NOT NULL,
    name VARCHAR2(100) NOT NULL,
    status VARCHAR2(20) DEFAULT 'ACTIVE',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 인덱스 생성
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_status ON users(status);

```


## 11. ViewResolver (뷰 이름 → 템플릿 파일)

**역할**: 컨트롤러가 반환한 논리적 뷰 이름을 실제 템플릿 파일로 변환

ViewResolver는 컨트롤러에서 반환된 뷰 이름을 실제 템플릿 파일의 경로로 변환합니다:

### Thymeleaf ViewResolver 설정


```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Bean
    public ThymeleafViewResolver thymeleafViewResolver() {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(templateEngine());
        resolver.setCharacterEncoding("UTF-8");
        resolver.setOrder(1);
        return resolver;
    }
}

```


### 뷰 이름 매핑 예시:

- 논리적 뷰 이름: `"user/detail"`
    
- 실제 파일 경로: `/templates/user/detail.html`
    

## 12. View (Thymeleaf 템플릿)

**역할**: 모델 데이터를 HTML로 렌더링하는 프레젠테이션 계층

Thymeleaf는 서버 사이드 Java 템플릿 엔진으로, 모델 데이터를 동적 HTML로 변환합니다:

### 주요 기능:

- **표현식 처리**: `${user.name}`, `#{message.key}` 등의 표현식 해석
    
- **조건부 렌더링**: `th:if`, `th:unless`를 통한 조건부 표시
    
- **반복 처리**: `th:each`를 통한 리스트 데이터 반복
    
- **폼 바인딩**: `th:object`, `th:field`를 통한 폼 데이터 바인딩
    


```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title th:text="#{page.user.title}">사용자 상세</title>
</head>
<body>
    <div th:object="${user}">
        <h1 th:text="*{name}">사용자 이름</h1>
        <p th:text="*{email}">이메일</p>
        <p th:text="*{#temporals.format(createdAt, 'yyyy-MM-dd HH:mm')}">가입일</p>
        
        <!-- 조건부 표시 -->
        <div th:if="*{status == 'ACTIVE'}" class="badge-active">
            활성 사용자
        </div>
        
        <!-- 반복 처리 -->
        <ul th:if="${user.orders}">
            <li th:each="order : ${user.orders}" 
                th:text="${order.orderNumber}">주문번호</li>
        </ul>
    </div>
</body>
</html>

```


## 13. HTTP Response (렌더링된 HTML)

**역할**: 클라이언트에게 전달되는 최종 응답

Thymeleaf 템플릿 엔진에 의해 렌더링된 HTML이 HTTP 응답으로 클라이언트에게 전달됩니다:

### 응답 구성 요소:

- **HTTP 상태 코드**: 200 OK, 404 Not Found, 500 Internal Server Error 등
    
- **응답 헤더**: Content-Type, Cache-Control, Set-Cookie 등
    
- **응답 본문**: 렌더링된 HTML 콘텐츠
    

```text
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 1234
Cache-Control: no-cache
Set-Cookie: JSESSIONID=ABC123; Path=/; HttpOnly

<!DOCTYPE html>
<html>
<head>
    <title>사용자 상세</title>
</head>
<body>
    <div>
        <h1>홍길동</h1>
        <p>hong@example.com</p>
        <p>2024-01-15 14:30</p>
        <div class="badge-active">활성 사용자</div>
    </div>
</body>
</html>

```


## 전체 흐름의 특징

### 장점:

- **관심사의 분리**: 각 계층이 명확한 책임을 가짐
    
- **재사용성**: 서비스와 매퍼는 여러 컨트롤러에서 재사용 가능
    
- **테스트 용이성**: 각 계층을 독립적으로 테스트 가능
    
- **유지보수성**: 변경 사항이 특정 계층에 국한됨
    

### 성능 고려사항:

- **커넥션 풀 관리**: 데이터베이스 연결 최적화
    
- **캐싱 전략**: Redis, EhCache 등을 통한 성능 향상
    
- **비동기 처리**: `@Async`를 통한 비동기 작업 처리
    

이러한 계층화된 아키텍처를 통해 Spring Boot 애플리케이션은 확장 가능하고 유지보수가 용이한 웹 애플리케이션을 구축할 수 있습니다.