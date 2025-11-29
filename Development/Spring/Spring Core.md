---
tags:
  - Spring/Core
---

## Spring core 만으로 일체의 어노테이션 없이 DI 를 구현하려면 어떻게 해야 할까요?

### SmartInitializingSingleton 의 afterSingletonsInstantiated 을 구현

```java
@Component
@Lazy(false)
public class AfterAllSingletonsHook implements SmartInitializingSingleton {

    private final ApplicationContext ctx;

    public AfterAllSingletonsHook(ApplicationContext ctx) {
        this.ctx = ctx;
    }

    @Override
    public void afterSingletonsInstantiated() {
        CustomService customService = new CustomService();

        UserService userService = ctx.getBean(UserService.class);
        userService.setCustomService(customService);
    }
}
```

-> SmartInitializingSingleton.afterSingletonsInstantiated()는 SpringApplication.run() 내부의
ApplicationContext.refresh() 과정 중, finishBeanFactoryInitialization 단계에서
“모든 일반 싱글톤이 사전 인스턴스화된 직후”에 호출되는 훅을 캡처.
즉, ContextRefreshedEvent가 publish되기 “직전”이며,
Boot의 ApplicationStartedEvent와 Runner, 그리고 ApplicationReadyEvent보다 이전 시점.

#### 수명주기에서의 위치

- ApplicationContext 리프레시 과정에는 finishBeanFactoryInitialization(...) 단계가 있으며, 이 단계에서 “남아 있는 싱글톤 초기화 마무
리”가 진행된다(메서드 시그니처와 역할은 클래스 Javadoc/메서드 목록으로 명시).[](https://docs.spring.io/spring-framework/docs/current
/javadoc-api/org/springframework/context/support/AbstractApplicationContext.html)​
    → 쉬운 말: refresh()의 막바지에 finishBeanFactoryInitialization가 호출되며, 여기서 싱글톤 초기화가 마무리된다.[](https://docs.sp
ring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/support/AbstractApplicationContext.html)​

- DefaultListableBeanFactory.preInstantiateSingletons()는 “Ensure that all non‑lazy‑init singletons are instantiated”로 설명되며, no
n‑lazy 싱글톤들을 실제로 인스턴스화하는 루프를 돈다.[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springfr
amework/beans/factory/support/DefaultListableBeanFactory.html)​
    → 쉬운 말: lazy가 아닌 싱글톤들을 전부 실제 객체로 만든다.[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/or
g/springframework/beans/factory/support/DefaultListableBeanFactory.html)​

- SmartInitializingSingleton.afterSingletonsInstantiated는 바로 이 “싱글톤 사전 인스턴스화(pre‑instantiation) 단계의 끝”에서 호출되
는 것으로 문서화되어 있어, 컨테이너의 finishBeanFactoryInitialization → preInstantiateSingletons 흐름이 끝날 무렵 실행되는 후처리 콜
백이라 보면 된다.[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/support/Defau
ltListableBeanFactory.html)​
    → 쉬운 말: preInstantiateSingletons가 non‑lazy 싱글톤들을 다 만든 다음, 그 끝자락에 SmartInitializingSingleton 콜백이 불린다고
이해하면 정확하다.[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/support/AbstractAp
plicationContext.html)​

쉽게 말해서

근데 이건 @Component 와 @Override 를 사용하기 때문에 어떻게 보면 어노테이션을 사용한다고 볼 수 있다.

그리고 Annotation 을 쓰길래 실제로 테스트해보진 않음.


### Hook 을 main function 에서 설정.



```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
@Slf4j
public class UserController {

    private final UserService userService;



    /**
     * CustomService 테스트 API
     * GET /api/users/hello     ** @return CustomService의 hello() 메서드 실행 결과
     */
    @GetMapping("/hello")
    public ResponseEntity<String> getHelloMessage() {
        log.info("GET /api/users/hello - CustomService hello 메서드 호출 요청");

        String message = executeCustomServiceHello();

        return buildHelloSuccessResponse(message);
    }

    ....
}
```

```java
@Service
@RequiredArgsConstructor
@Slf4j
public class UserService {

    private final UserRepository userRepository;

    public CustomService customService;

    public void setCustomService (CustomService customService) {
        this.customService = customService;
    }

    public String customServiceHello () {
        return customService.hello();
    }

    ....
}
```

```java
package com.example.demo.service;

public class CustomService {
    public String hello () {
        return "                                       Hello!";
    }
}
```


![[Screenshot 2025-10-15 at 1.16.08 PM.png]]

일단 이 상태에서 안되는거 확인.




```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(DemoApplication.class);

        app.addInitializers(ctx -> {
            if (ctx instanceof GenericApplicationContext gac) {
                gac.registerBean(SmartInitializingSingleton.class, () -> () -> {

                    UserService userService = ctx.getBean(UserService.class);
                    userService.setCustomService(new CustomService());

                });
            }
        });

        app.run(args);
    }
}
```

적용후.


![[Screenshot 2025-10-15 at 1.27.06 PM.png]]

성공.



컨텍스트가 만들어지기 전에 SpringApplication에 훅을 심고(addInitializers),
run()이 컨텍스트를 생성·refresh하는 동안 그 훅이 실행되어
콜백 빈(SmartInitializingSingleton)을 등록하고,
모든 싱글톤 초기화 직후 그 콜백이 실행되면서 UserService를 조작하는 흐름.

#### 한 줄 요약

네 코드는 **람다(lambda) 표현식**으로 SmartInitializingSingleton 인터페이스를 "즉석에서 구현한 익명 객체"를 만들어 빈으로 등록한 것
이고, `extends`나 `implements` 키워드를 명시적으로 쓴 클래스를 따로 정의하지 않았다.[baeldung+1](https://www.baeldung.com/java-8-fun
ctional-interfaces)​

#### 코드 분해 설명

```java
gac.registerBean(SmartInitializingSingleton.class, () -> () -> {
    UserService userService = ctx.getBean(UserService.class);
    userService.setCustomService(new CustomService());
});

```

#### 1단계: registerBean 시그니처

- `registerBean(빈타입.class, Supplier<빈인스턴스>)` 형태로 동작한다.[geeksforgeeks+1](https://www.geeksforgeeks.org/java/lambda-exp
ressions-java-8/)​

- 여기서 `Supplier<SmartInitializingSingleton>`을 넘겨야 하는데, Supplier도 함수형 인터페이스(functional interface)라서 람다로 쓸 수
 있다.[baeldung+1](https://www.baeldung.com/java-8-functional-interfaces)​


#### 2단계: 첫 번째 람다 `() -> ...`

- 이건 `Supplier<SmartInitializingSingleton>`의 `get()` 메서드를 구현하는 람다다.[geeksforgeeks+1](https://www.geeksforgeeks.org/jav
a/lambda-expressions-java-8/)​

- "SmartInitializingSingleton 인스턴스를 어떻게 만들지"를 정의하는 부분이다.[baeldung+1](https://www.baeldung.com/java-8-functional-
interfaces)​


#### 3단계: 두 번째 람다 `() -> { ... }`

- 이건 SmartInitializingSingleton의 `afterSingletonsInstantiated()` 메서드를 구현하는 람다다.[spring+2](https://docs.spring.io/sprin
g-framework/docs/6.1.2/javadoc-api/org/springframework/beans/factory/SmartInitializingSingleton.html)​

- SmartInitializingSingleton도 추상 메서드가 하나뿐인 인터페이스(functional interface)라서 람다로 바로 구현 가능하다.[geeksforgeeks+
1](https://www.geeksforgeeks.org/java/lambda-expressions-java-8/)​


#### 전통 방식으로 풀어쓰면 (비교용)

```java
// 전통적으로 클래스로 구현하는 방식
class MyCallback implements SmartInitializingSingleton {
    @Override
    public void afterSingletonsInstantiated() {
        UserService userService = ctx.getBean(UserService.class);
        userService.setCustomService(new CustomService());
    }
}

gac.registerBean(SmartInitializingSingleton.class, () -> new MyCallback());

```


네 코드는 위의 `MyCallback` 클래스 정의를 **람다로 즉석 생성**한 것이다.[baeldung+1](https://www.baeldung.com/java-8-functional-inte
rfaces)​

#### Functional Interface(함수형 인터페이스)란

- "추상 메서드가 딱 하나만 있는 인터페이스"를 말한다 (default/static 메서드는 여러 개 있어도 무방).[geeksforgeeks+1](https://www.gee
ksforgeeks.org/java/lambda-expressions-java-8/)​

- 자바는 이런 인터페이스를 **람다 표현식으로 바로 구현**할 수 있게 해준다 (익명 클래스 문법을 간소화).[baeldung+1](https://www.baeld
ung.com/java-8-functional-interfaces)​

- 대표 예시: `Runnable`, `Callable`, `Comparator`, `Supplier`, 그리고 SmartInitializingSingleton도 여기 속한다.[spring+2](https://do
cs.spring.io/spring-framework/docs/6.1.2/javadoc-api/org/springframework/beans/factory/SmartInitializingSingleton.html)​


#### 왜 extends/implements가 안 보이나

- 람다는 컴파일러가 "어떤 인터페이스의 유일한 추상 메서드를 구현하는구나"라고 **타입 추론(type inference)**으로 자동 판단한다.[geeks
forgeeks+1](https://www.geeksforgeeks.org/java/lambda-expressions-java-8/)​

- 따라서 `implements SmartInitializingSingleton`을 명시하지 않아도, 컴파일러가 "이 람다는 SmartInitializingSingleton 구현체다"라고
알아서 연결해준다.[baeldung+1](https://www.baeldung.com/java-8-functional-interfaces)​


#### 초등학생 설명 (쉬운 비유)

- 전통 방식: "나는 OO팀 선수입니다"라고 쓴 유니폼을 입고 경기장에 가는 것 (클래스 정의 + implements 명시).[geeksforgeeks+1](https://
www.geeksforgeeks.org/java/lambda-expressions-java-8/)​

- 람다 방식: 감독이 "이 사람은 OO팀"이라고 이미 알고 있어서, 유니폼 없이도 알아서 팀에 넣어주는 것 (타입 추론으로 구현 생략).[baeldu
ng+1](https://www.baeldung.com/java-8-functional-interfaces)​


## spring 의 proxy pattern 에 대하여

(spring 5 Design pattern > Consideration of Structural and Behavioral patterns > Examining the core design patterns > Structural Des
ign patterns > Proxy Design )


Proxy design 은 어떤 클래스의 객체를 제공하는데, 이 객체는 다른 클래스의 기능을 가진 객체이다.
실제 객체를 직접 사용하지 않고, 대리인 객체를 만들어 제공하는 패턴.

이 패턴은 GOF design patterns 의 structural design 패턴 (구조적 디자인 패턴) 에 속한다.

GOF ? -> Gang of Four

- GOF(Gang of Four)**는 네 명의 저자(Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides)를 가리키는 말.
- 이 네 명이 쓴 책이 바로 **"Design Patterns: Elements of Reusable Object-Oriented Software"**(1994)이고, 이 책에서 23가지의 대표적
인 디자인 패턴을 정리함.
- 이 패턴들은 소프트웨어 설계에서 반복적으로 등장하는 문제를 해결하기 위한 베스트 프랙티스(best practice)로, 객체지향 프로그래밍에서
 아주 널리 쓰인다.


GOF 가 정의한 23개의 디자인 패턴 중, 프록시 패턴은 '클래스와 객체들의 구조' 를 설계하는데 사용되는 패턴

### 프록시 패턴의 핵심 목적

- 실제 객체 (Real Object) 대신 대리인 (proxy) 를 외부에 노출 시키는 것
- 하지만 이 대리인은 실제 객체와 똑같은 기능을 제공해야 함
- 클라이언트(외부) 는 프록시를 사용하는지, 실제 객체를 사용하는지 구별할 수 없어야 한다.


### 예시 코드

```java
// 1. Subject (공통 인터페이스)
public interface Account {
    void accountType();
}

// 2. RealSubject (실제 구현체)
public class SavingAccount implements Account {
    public void accountType() {
        System.out.println("SAVING ACCOUNT");
    }
}

// 3. Proxy (대리인)
public class ProxySavingAccount implements Account {
    private Account savingAccount;  // 실제 객체 참조

    public void accountType() {
        // 실제 객체가 없으면 생성 (lazy initialization)
        if (savingAccount == null) {
            savingAccount = new SavingAccount();
        }
        // 실제 객체에게 작업 위임
        savingAccount.accountType();
    }
}
```

```java
Account account = new ProxySavingAccount();  // 프록시 사용
account.accountType();  // 하지만 실제 기능은 동일!
```

#### 1. **실제 객체를 외부로부터 숨김 (Hide the actual object)**

- 클라이언트는 실제 구현체를 몰라도 됨

#### 2. **성능 향상 (Improve performance)**

- **Lazy initialization** (필요할 때만 생성)
- **Caching** (결과를 캐시해서 재사용)

#### 3. **접근 제어 (Control access)**

- **보안 검사** (권한이 있는지 확인)
- **로깅** (메서드 호출 기록)


### Spring에서 프록시가 사용되는 곳:

1. **Spring AOP** - 트랜잭션, 보안, 로깅 등 횡단 관심사(cross-cutting concerns) 적용
2. **Transaction Management** - `@Transactional` 어노테이션
3. **Security** - `@Secured`, `@PreAuthorize` 등
4. **Remote Services** - RMI, HTTP Invoker
