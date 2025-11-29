---
tags:
  - Spring/Web
---

## Filter vs Interceptor

### Jsp Programming 책에서 말하는 Filter

HTTP 요청과 응답을 변경할 수 있는 재사용 가능한 클래스.
jsp/서블릿을 실행하기 전에 요청이 올바른지, 자원에 접근할 수 있는 권한이 있는지 여부를 미리 처리할 수 있음.

### 사전 지식

Tomcat = 서블릿 컨테이너 + 웹 서버
> "Tomcat is composed mainly of three components: (1) **Catalina, a servlet container**, (2) **Coyote, a web server**, and
> (3) Jasper, a Jakarta Server Pages processor."

- 웹 서버
        - HTTP 프로토콜을 이해하고 처리하는 서버, 정적파일 (HTML, CSS, js, 이미지 등)을 그대로 전달하는게 주 역할
        - Tomcat 은 스레드풀을 사용함 (따라서 jsp, spring 모두 스레드풀을 사용함)
        - Apache HTTP server, Nginx 등도 Thread Pool 을 쓰지만 방식이 다름
- 서블릿 컨테이너
        - 서블릿 생명주기 관리 (init, service - 요청 처리, destory - 정리)
        - 요청 라우팅 - HTTP request 를 적절한 servlet 으로 매핑
        - 멀티 스레드 처리 - 스레드 풀에서부터 스레드를 가져와 Request 처리
        - 세션 관리
        - 보안 - 인증, 권한 체크

톰캣은 WAS 가 아니다 (Web Application Server).




Spring Boot 에서
- jar 파일 : 내장 Tomcat 포함
          java -jar myapp.jar 로 실행
- war (web archive) 파일 : Tomcat 제외하고 패키징,
          class 파일 + HTML + JSP + 설정 파일 등을 담은 ZIP 압축 파일 (확장자가 .war 임)


SpringBoot 가 일반적으로 사용하는 ApplicationContext 타입
-> `AnnotationConfigServletWebServerApplicationContext`

Spring Boot는 `WebApplicationType`에 따라 자동으로 적절한 ApplicationContext를 선택
- **SERVLET** 타입 (spring-boot-starter-web 사용 시) → `AnnotationConfigServletWebServerApplicationContext` 생성[](https://docs.spri
ng.io/spring-boot/reference/web/servlet.html)​
- **REACTIVE** 타입 (WebFlux 사용 시) → `AnnotationConfigReactiveWebServerApplicationContext` 생성[](https://stackoverflow.com/quest
ions/62229265/what-is-a-default-spring-boot-application-context)​
- **NONE** 타입 (웹 기능 없는 일반 앱) → `AnnotationConfigApplicationContext` 생성[](https://stackoverflow.com/questions/62229265/wh
at-is-a-default-spring-boot-application-context)​

java.lang.Object
        org.springframework.core.io.DefaultResourceLoader
                org.springframework.context.support.AbstractApplicationContext
                        org.springframework.context.support.GenericApplicationContext
                                org.springframework.web.context.support.GenericWebApplicationContext
                                        org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext
                                                org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplica
tionContext

이 중에
GenericWebApplicationContext class 가 ConfigurableWebApplicationContext interface 를 implement 하고,
ConfigurableWebApplicationContext interface 가 WebApplicationContext interface 를 extends 한다.

docs.spring.io 에
> `DispatcherServlet` expects a `WebApplicationContext` (an extension of a plain `ApplicationContext`) for its own configuration.


- Spring 의 DispatcherServlet 안에는 2가지 타입의 WebApplicationContext 이 있다.

![[Screenshot 2025-10-29 at 12.38.27 PM.png|600]]


- jsp 는 jsp 페이지마다 서블릿을 생성하고, 이 서블릿은 singleton 으로 하나만 존재하며,
이 서블릿들은 상태가 없는한 할당되는 메모리 크기가 매우 작다.
그리고 요청이 올때마다 이를 재사용해서 비효율적이지 않다.
jsp 의 servlet 은 Heap 공간에 생성됨 (spring은 모름)


- spring 의 경우 servlet 을 가리키는 것은 DispatcherServlet 이며 이는 보통 하나이며,
이 크기는 매우 큰데
DispatcherServlet 의 크기 자체는 별로 안크지만, WebApplicationContext 의 크기가 매우 크기 때문에
이를 포함하는 DispatcherServlet 자체도 자연스럽게 커지게 된다.

왜냐하면 WebApplicationContext 가 Bean 을 포함하기 때문
(WebApplicationContext 는 ApplicationContext 을 extends 한다. 즉 Bean 의 생명주기 관리에 참여할수도 있음(may))

이렇게 보면, spring 은 jsp 의 servlet 자체를 엄청나게 확장한 것으로 생각할 수 있다.

- spring 에는 여러개의 DispatcherServlet 이 존재할 수 있다 (jsp 처럼)
        여러 개의 DispatcherServlet이 필요한 경우는 매우 특수한 상황입니다:[](https://stackoverflow.com/questions/23049736/working-w
ith-multiple-dispatcher-servlets-in-a-spring-application)​​
        - UI 요청과 RESTful API 요청을 완전히 분리해야 하는 경우
        - 완전히 다른 설정(view resolver, interceptor 등)이 필요한 경우
        - 레거시 시스템과의 통합 등
**요약**: 가능하긴 하지만, 실제로는 거의 모든 애플리케이션에서 하나의 DispatcherServlet만 사용합니다.
이것이 Front Controller 패턴의 원칙에도 부합하고, Spring MVC의 표준 구성입니다.


자, 이 이해를 바탕으로...

### Filter ?

Filter 는 Servlet Container 레벨에서 동작하는 Java Class 이다.
Request, Response 가 서블릿(jsp 는 페이지마다, spring에는 보통 하나만 존재하는) 에 도착하기 전 후로 전처리와 후처리를 수행한다.

그러니까 Spring 으로 치면 Request 가 DispatcherServlet 에 닿기 전에 pre-Processing 하고,
Response 가 DispatchServlet 으로부터 나와 client 에 닿기 전에 post-processing 하는 클래스를 말함.

(Spring MVC 에서의 실행 흐름)
```java
Client Request
    ↓
Filter Chain
    ↓
['보통 하나'의 DispatcherServlet]
    ↓
ApplicationContext
    ↓
Handler Mapping (어떤 Controller로 보낼지 결정) -> 얘도 Bean 이다.
    ↓
Controller (예: @GetMapping("/user"))
    ↓
Service/Repository
    ↓
Controller가 Model과 View 이름을 리턴
    ↓
ViewResolver (View 이름 → 실제 JSP 파일 경로 매핑)
    ↓
[해당 JSP의 서블릿] ← 여기서 JSP 서블릿이 실행됨
    ↓
렌더링된 HTML이 클라이언트에게 리턴
```

어쨌든 Servlet Container 에서 관리되므로 전통적인 Filter 에서는 Spring 의 컨텍스트와 상관없이 동작한다.

- 전통적 FIlter 구현 (web.xml)
```xml
<filter>
    <filter-name>LogFilter</filter-name>
    <filter-class>com.example.LogFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>LogFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

SpringBoot 은 Bean 을 사용해서 Filter 를 설정할 수 있다.

----
(이 부분은 다시 다듬기 필요)
**전통적인 Java 웹 애플리케이션 (WAR 배포)**

- 외부 Servlet Container (Tomcat, Jetty 등)에 WAR 파일을 배포.
- `web.xml`에 Filter와 DispatcherServlet을 등록.
- 이 경우, Servlet Container가 주도권을 가지고 있다.​

**Spring Boot (Embedded Container)**

- Spring Boot는 **Embedded Servlet Container**를 사용해. 즉, Tomcat이 애플리케이션 안에 포함되어 있음.[](https://docs.spring.io/spri
ng-boot/reference/web/servlet.html)​
- Spring Boot가 애플리케이션을 시작할 때, **Spring Boot가 Servlet Container를 생성하고 설정**함. 그래서 주도권이 역전됨.[](https://m
osy.tech/blog/spring-boot-autoconfiguration/)​
- 이 과정에서 Spring Boot는 `ServletWebServerFactory`를 통해 Servlet Container를 초기화하고, Filter와 Servlet을 프로그래밍 방식으로
(xml 이 아니라) 등록할 수 있음.

Spring만 사용하면,
Spring MVC (Spring Boot 아님)에서는 `DelegatingFilterProxy`라는 특수한 Filter를 `web.xml`에 등록.
이 녀석이 Servlet Container와 Spring ApplicationContext를 연결해주는 다리 역할을 한다.[](https://stackoverflow.com/questions/6725234
/whats-the-point-of-spring-mvcs-delegatingfilterproxy)​

-  동작 방식
1. `web.xml`에 `DelegatingFilterProxy`를 등록.[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframewor
k/web/filter/DelegatingFilterProxy.html)​
2. Spring Context에 실제 Filter를 Bean으로 등록.[](https://stackoverflow.com/questions/6725234/whats-the-point-of-spring-mvcs-delega
tingfilterproxy)​
3. `DelegatingFilterProxy`가 요청을 받으면, Spring Context에서 해당 Bean을 찾아서 위임(delegate).[](https://docs.spring.io/spring-se
curity/reference/servlet/architecture.html)​

---

### Interceptor ?

jsp 에는 없는 개념이다.

Spring 에는 두 가지 인터셉터 종류가 있다.

하나는 Spring MVC 에서 DispatcherServlet 에서 Handler 로 갈때 요청을 가로채는 interceptor 와
AOP interceptor 가 있다.

먼저 Spring 이 Bean을 생성할때, 해당 Bean 에 AOP 가 적용되는 지 확인.
적용대상이면, Bean 을 Proxy 로 감싸서 ApplicationContext 에 등록. (이 작업은 이제 Runtime 에 일어난다)
이렇게 등록되고 나면, 해당 Bean 에 대해서 `getBean()` 으로 꺼내온 빈은 사실 Proxy 객체이다.

그래서 proxy 로 감싸진 빈의 메서드가 호출되면 (물론 메서드마다 프록시 호출여부를 설정할 수 있다),
Proxy 클래스가 먼저 실행된다.
이때 Proxy class 는 해당 메서드의 실행전, 후로 동작을 수행할 수 있다.
(Before, Around, After 등의 advice 가 그 것)

**두 가지 Proxy 방식**
- **JDK Dynamic Proxy**: 인터페이스 기반. 인터페이스를 구현한 Proxy 클래스를 런타임에 생성.​
- **CGLIB Proxy**: 클래스 기반. 원본 클래스를 상속받은 Subclass를 런타임에 생성.​


### Filter vs Interceptor

먼저 3가지 요소를 비교해야 한다고 생각한다.
Filter , HanlderInterceptor (Spring MVC의), AOP Interceptor

Filter 와 Handler interceptor 는 HTTP 요청 레벨에서 동작한다.
Filter는 Servlet Container ,  Handler interceptor 는 DispatcherServlet 레벨에서 가로챌뿐
요청/응답을 가로채는것은 동일함.

Handler Interceptor 는 DispatcherServlet 이 직접 Interceptor chain 을 실행함

```java
1. mappedHandler.applyPreHandle() → 모든 Interceptor의 preHandle() 실행
2. HandlerAdapter.handle() → Controller 실행
3. mappedHandler.applyPostHandle() → 모든 Interceptor의 postHandle() 실행 (역순)
4. View 렌더링
5. mappedHandler.triggerAfterCompletion() → 모든 Interceptor의 afterCompletion() 실행 (역순)
```

그러나 이 둘과 확연하게 AOP Interceptor 는 구분되는 것이
AOP Interceptor 는 method 레벨에서 동작한다.
HTTP 와 무관하고,
모든 SpringBean 에 대해서 메서드를 가로챌 수 있다.
Proxy 기반으로 동작한다.

여기서 Proxy 란
Proxy는 **원본 Bean(Target Object)을 감싸서**, 원본 Bean 호출시
원본 메서드 호출을 가로채고 추가 로직(Advice)을 실행한 뒤, 원본 메서드를 호출하는 객체.


## @RestController

### @RestController vs @Controller

`@RestController` = `@Controller` + `@ResponseBody`

#### @ResponseBody
- 메서드의 반환값을 **View Resolver**를 거치지 않고 **HTTP Response Body**에 직접 쓴다.
- 객체 반환 시 `HttpMessageConverter` (보통 Jackson)가 JSON 등으로 직렬화한다.

#### ResponseEntity 안 써도 되는 이유
- `@RestController`나 `@ResponseBody`를 사용하면 객체를 반환하기만 해도 자동으로 JSON 변환 및 200 OK 응답이 생성됨.
- 헤더나 상태 코드를 세밀하게 제어해야 할 때만 `ResponseEntity`를 사용하면 됨.