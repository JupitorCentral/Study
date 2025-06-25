- [ ] Flow of Client Request ➕ 2025-06-17 

1. Client Request
    ↓
2. Tomcat (Servlet Container)
    ↓
3. Spring Security Filter Chain
    ↓
4. DispatcherServlet (Front Controller)
    ↓
5. HandlerMapping (URL → Controller mapping)
    ↓
6. HandlerAdapter (Parameter Binding)
    ↓
7. Controller (@RequestMapping)
    ↓
8. Service (@Service, Business Logic)
    ↓
9. Mapper (@Mapper, Data Access)
    ↓
10. Database (Oracle)
    ↓
11. ViewResolver (View name -> Template File)
    ↓
12. View (Thymeleaf Template)
    ↓
13. HTTP Response (Rendered HTML)


## 1. Client Request

**Role**: The starting point for all requests to the web application.

The client (browser, mobile app, API client, etc.) generates an HTTP request. This request includes the following information:

- **HTTP Method** (GET, POST, PUT, DELETE, etc.)
- **URL Path** (`/users/123`, `/api/products`, etc.)
- **Header Information** (Content-Type, Authorization, Accept, etc.)
- **Request Body** (for POST/PUT requests)
- **Query Parameters** (`?page=1&size=10`)

## 2. Tomcat (Servlet Container)

**Role**: A servlet container that runs Java web applications.

Apache Tomcat is a Java EE web application server that performs the following core functions:

- **Receives HTTP Requests**: Accepts incoming HTTP requests from clients.
- **Thread Management**: Processes each request in a separate thread.
- **Servlet Lifecycle Management**: Manages the creation, initialization, execution, and destruction of servlets.
- **Session Management**: Creates and manages HTTP sessions.
- **Static Resource Handling**: Directly serves static files like CSS, JavaScript, and images.

In Spring Boot, an embedded Tomcat is included by default, allowing you to use it without separate installation or configuration.

## 3. Spring Security Filter Chain

**Role**: A filter chain responsible for handling security-related processes.

Spring Security operates based on servlet filters and performs security checks before the request reaches the application logic:

### Key Filters:

- **SecurityContextPersistenceFilter**: Loads and saves the security context from/to the HTTP session.
- **UsernamePasswordAuthenticationFilter**: Handles form-based login authentication.
- **BasicAuthenticationFilter**: Handles HTTP Basic authentication.
- **JwtAuthenticationFilter**: Handles JWT token-based authentication (custom).
- **AuthorizationFilter**: Performs authorization checks and access control.
- **ExceptionTranslationFilter**: Handles security exceptions.

<!-- end list -->

Java

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

**Role**: The central control point of Spring MVC.

It implements the Front Controller pattern, receiving all HTTP requests and routing them to the appropriate controller:

- **Request Reception**: Centrally accepts all HTTP requests.
- **Handler Mapping**: Finds the controller that matches the request URL.
- **Exception Handling**: Manages global exception handling.
- **Response Generation**: Delivers the final response to the client.

In Spring Boot, it is auto-configured to map to the `/*` pattern, processing all requests.

## 5. HandlerMapping (URL → Controller Mapping)

**Role**: Maps the request URL to the appropriate controller method.

It analyzes the requested URL and HTTP method to determine which controller should handle the request:

### Key Mapping Methods:

- **@RequestMapping**: The basic mapping annotation.
- **@GetMapping**: Maps GET requests.
- **@PostMapping**: Maps POST requests.
- **@PathVariable**: Maps URL path variables.
- **@RequestParam**: Maps query parameters.

<!-- end list -->

Java

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")  // GET /api/users/123
    public User getUser(@PathVariable Long id) {
        // This method is selected by the HandlerMapping.
    }
}
```

## 6. HandlerAdapter (Parameter Binding)

**Role**: Executes the controller method and handles parameter binding.

It actually calls the selected controller method and converts HTTP request data into method parameters:

### Key Functions:

- **Parameter Binding**: Converts HTTP request data into Java objects.
- **Data Validation**: Validates input data using the `@Valid` annotation.
- **Type Conversion**: Converts strings to appropriate Java types.
- **Method Invocation**: Calls the actual controller method.

<!-- end list -->

Java

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(
    @Valid @RequestBody UserCreateRequest request,  // JSON → Java object conversion
    @RequestHeader("Authorization") String token,   // Header binding
    @RequestParam(defaultValue = "false") boolean sendEmail // Query parameter binding
) {
    // The HandlerAdapter appropriately binds all parameters.
}
```

## 7. Controller (@RequestMapping)

**Role**: The web layer that receives HTTP requests and calls business logic.

The controller receives web requests, calls the appropriate services, and generates a response:

### Key Responsibilities:

- **Request Validation**: Validates the input data.
- **Service Invocation**: Calls the service layer responsible for business logic.
- **Response Generation**: Converts the processing result into an HTTP response.
- **Exception Handling**: Handles controller-level exceptions.

<!-- end list -->

Java

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

## 8. Service (@Service, Business Logic)

**Role**: The service layer that handles the core business logic of the application.

The service layer implements the actual business rules and logic:

### Key Responsibilities:

- **Business Logic Implementation**: Applies domain rules and policies.
- **Transaction Management**: Ensures data consistency through `@Transactional`.
- **Data Processing**: Combines and processes information from multiple data sources.
- **External Service Integration**: Communicates with other systems.

<!-- end list -->

Java

```java
@Service
@Transactional(readOnly = true)
public class UserService {
    
    private final UserMapper userMapper;
    private final EmailService emailService;
    
    @Transactional
    public User createUser(UserCreateRequest request) {
        // Validate business rules
        validateUserCreation(request);
        
        // Create user
        User user = User.builder()
                .email(request.getEmail())
                .name(request.getName())
                .status(UserStatus.ACTIVE)
                .createdAt(LocalDateTime.now())
                .build();
                
        userMapper.insertUser(user);
        
        // Send welcome email (business logic)
        emailService.sendWelcomeEmail(user);
        
        return user;
    }
    
    private void validateUserCreation(UserCreateRequest request) {
        if (userMapper.existsByEmail(request.getEmail())) {
            throw new DuplicateEmailException("This email already exists.");
        }
    }
}
```

## 9. Mapper (@Mapper, Data Access)

**Role**: The data access layer responsible for interacting with the database.

It executes SQL queries through MyBatis's `@Mapper` interface and maps the results to Java objects:

### Key Functions:

- **SQL Mapping**: Defines SQL queries using XML or annotations.
- **Result Mapping**: Automatically converts database results into Java objects.
- **Parameter Binding**: Converts Java objects into SQL parameters.
- **Dynamic Queries**: Generates dynamic SQL based on conditions.

<!-- end list -->

Java

``` java
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
    
    // Example of a dynamic query (using an XML mapping file)
    List<User> findUsers(UserSearchCondition condition);
}
```

## 10. Database (Oracle)

**Role**: The persistent storage for application data.

Oracle Database is an enterprise-grade relational database that provides the following features:

### Key Features:

- **ACID Transactions**: Guarantees data consistency and integrity.
- **Concurrency Control**: Manages simultaneous access from multiple users.
- **Backup and Recovery**: Prevents data loss.
- **Performance Optimization**: Improves query performance through indexes, partitioning, etc.

<!-- end list -->

SQL

``` SQL
-- Example User Table
CREATE TABLE users (
    id NUMBER PRIMARY KEY,
    email VARCHAR2(255) UNIQUE NOT NULL,
    name VARCHAR2(100) NOT NULL,
    status VARCHAR2(20) DEFAULT 'ACTIVE',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- Create Indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_status ON users(status);
```

## 11. ViewResolver (View Name → Template File)

**Role**: Converts the logical view name returned by the controller into an actual template file.

The ViewResolver translates the view name returned from the controller into the path of an actual template file:

### Thymeleaf ViewResolver Configuration

Java

```
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

### View Name Mapping Example:

- Logical View Name: `"user/detail"`
- Actual File Path: `/templates/user/detail.html`

## 12. View (Thymeleaf Template)

**Role**: The presentation layer that renders model data into HTML.

Thymeleaf is a server-side Java template engine that converts model data into dynamic HTML:

### Key Features:

- **Expression Handling**: Parses expressions like `${user.name}` and `#{message.key}`.
- **Conditional Rendering**: Displays content conditionally using `th:if` and `th:unless`.
- **Iteration**: Iterates over list data using `th:each`.
- **Form Binding**: Binds form data using `th:object` and `th:field`.

<!-- end list -->

HTML

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title th:text="#{page.user.title}">User Details</title>
</head>
<body>
    <div th:object="${user}">
        <h1 th:text="*{name}">User Name</h1>
        <p th:text="*{email}">Email</p>
        <p th:text="*{#temporals.format(createdAt, 'yyyy-MM-dd HH:mm')}">Join Date</p>
        
        <div th:if="*{status == 'ACTIVE'}" class="badge-active">
            Active User
        </div>
        
        <ul th:if="${user.orders}">
            <li th:each="order : ${user.orders}" 
                th:text="${order.orderNumber}">Order Number</li>
        </ul>
    </div>
</body>
</html>
```

## 13. HTTP Response (Rendered HTML)

**Role**: The final response delivered to the client.

The HTML rendered by the Thymeleaf template engine is delivered to the client as an HTTP response:

### Response Components:

- **HTTP Status Code**: 200 OK, 404 Not Found, 500 Internal Server Error, etc.
- **Response Headers**: Content-Type, Cache-Control, Set-Cookie, etc.
- **Response Body**: The rendered HTML content.

<!-- end list -->

Plaintext

```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 1234
Cache-Control: no-cache
Set-Cookie: JSESSIONID=ABC123; Path=/; HttpOnly

<!DOCTYPE html>
<html>
<head>
    <title>User Details</title>
</head>
<body>
    <div>
        <h1>John Doe</h1>
        <p>john@example.com</p>
        <p>2024-01-15 14:30</p>
        <div class="badge-active">Active User</div>
    </div>
</body>
</html>
```

## Characteristics of the Overall Flow

### Advantages:

- **Separation of Concerns**: Each layer has a clear responsibility.
- **Reusability**: Services and mappers can be reused by multiple controllers.
- **Testability**: Each layer can be tested independently.
- **Maintainability**: Changes are localized to a specific layer.

### Performance Considerations:

- **Connection Pool Management**: Optimizes database connections.
- **Caching Strategy**: Improves performance using Redis, EhCache, etc.
- **Asynchronous Processing**: Handles asynchronous tasks using `@Async`.