---
tags:
  - Java/Annotation
---

## Annotation


### Purpose

#### information

컴파일러한테 정보를 알려주거나

#### task

컴파일 할떄와 deployment 시 작업을 지정

#### when execution

실행시 별도 처리를 위해


### 정해져 있는 Annotation

#### @Override

Override 의 명시적선언.
-> 오버라이딩때 개발자가 실수로 뭐하나 뺴뜨릴 수 있는데, 그럴때 컴파일러한테 알려달라고 하는 용도

#### @Deprecated

더이상 사용하지 않을 것이라는 것을 누군가에게 경고해주길 컴파일러에게 요청

#### @SurpressWarnings

해당 어노테이션이 사용된 메소드의 warning 메시지를 빌드시 미출력하게 함

@SurpressWarnings("deprecation")  -> Deprecated warning 을 없앤다.

### Meta Annotation - For defining my own Annotation

#### @Target

어노테이션을 어떤 곳에 쓸 것인가 ?
```java
@Target(ElementType.Method)
```

- CONSTRUCTOR 생성자 선언시
- FIELD
- LOCAL_VARIABLE 지역변수 선언시
- METHOD
- PACKAGE
- PARAMETER
- TYPE - 클래스, 인터페이스, enum 등 선언시

#### @Retention

얼마나 정보가 유지되는가 ?

```java
@Retention(RetentionPolicy.RUNTIME)
```

- SOURCE -> 컴파일시 사라짐
- CLASS -> 클래스 파일에 있는 어노테이션 정보가 컴파일러에 의해서 참조 가능. JVM 에서는 사라짐.
- RUNTIME -> 실행시 JVM 에 의해서 참조 가능


#### @Documented

해당 어노테이션에 대한 정보가 Javadocs (API) 문서에 포함된다는 것을 선언
-> 뭔소리야 ?

#### @Inherited

모든 자식 클래스에서 부모클래스의 어노테이션을 사용가능하다 -> 또 뭔소리야 ?

#### @interface
-> 어노테이션을 선언할 때 사용


### Defining an Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UserAnnotation {
    public int number();
    public String text() default "This is first annotation";
}
```

-> 얘도 클래스 비슷하게, 한 파일의 public  클래스와 같은 레벨에서 선언이 안된다.

-> 그런데 어노테이션에 값 선언이 가능하네 ... ?

### Checking variables defined inside Annotation

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.lang.reflect.Array;
import java.lang.reflect.Method;
import java.util.ArrayList;

public class Main {

    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface UserAnnotation {
        public int number();
        public String text() default "This is first annotation";
    }

    @UserAnnotation(number=1)
    public void method1 () {
    }
    @UserAnnotation(number=2, text = "Second")
    public void method2 () {

    }
    @UserAnnotation(number=3, text = "Third")
    public void method3 () {

    }

    public static void main(String[] args) throws Exception {
        Main mainObj = new Main();
        mainObj.checkAnnotations(Main.class);
    }

    public void checkAnnotations (Class useClass) {
        Method[] methods = useClass.getDeclaredMethods();
        for (Method method : methods) {
            UserAnnotation annotation = method.getAnnotation(UserAnnotation.class);
            if (annotation != null) {
                int number = annotation.number();
                String text = annotation.text();
                println(method.getName() + " - number : " + number + ", text : " + text);
            } else {
                println(method.getName() + " - annotation is empty");
            }
        }
    }

    public static void printf (String string, Object... args) { System.out.printf (string, args); }
    public static <T> void print (T obj) { System.out.print(obj + " "); }
    public static <T> void println (T obj) { System.out.println(obj); }
    public static void println() { System.out.println(); }
}
------------------

> Task :Main.main()
println - annotation is empty
println - annotation is empty
main - annotation is empty
print - annotation is empty
printf - annotation is empty
checkAnnotations - annotation is empty
method1 - number : 1, text : This is first annotation
method2 - number : 2, text : Second
method3 - number : 3, text : Third
```

-> java.lang.reflect 사용
reflextion api ?

### Inheritence of Annotation

상속 불가능. (enum 처럼)


#### @FunctionalInterface ?


```java
@Documented        // javadoc에 표시
@Retention(RUNTIME) // 런타임까지 어노테이션 정보 유지  
@Target(TYPE)      // 타입(클래스/인터페이스)에만 사용 가능
public @interface FunctionalInterface {}
```

Retension이 Runtime 이므로, 해당 타입의 어노테이션의 정보가 runtime 동안 유지된다.
그렇게 되면, 프레임워크/라이브러리가 해당 타입에 대해 처리할때 (프레임워크/라이브러리가 리플렉션으로 어노테이션을 읽음)
딸려있는 어노테이션의 정보에 따라 동작을 다르게 한다.
(Annotation 자체에도 값을 설정할 수 있다.)

리플렉션 ? 이거 많이 들어봤는데


#### @Documented ?

```java
(1)
If the annotation @Documented is present on the declaration of an annotation interface A, 
then any @A annotation on an element is considered part of the element's public contract. 

(2)
In more detail, when an annotation interface A is annotated with Documented, 
the presence and value of A annotations are a part of the public contract of the elements A annotates. 

(3)
Conversely, if an annotation interface B is not annotated with Documented, 
the presence and value of B annotations are not part of the public contract of the elements B annotates. 

(4)
Concretely, if an annotation interface is annotated with Documented, 
by default a tool like javadoc will display annotations of that interface in its output 
while annotations of annotation interfaces without Documented will not be displayed
```

- [ ] Concretely #EnglishWord ➕ 2025-09-22 
      in a definitive or conclusive way.
      구체적으로, 명확하게
      She explained her ideas concretely with examples.
		(그녀는 예시를 들어 구체적으로 설명했다).

- [ ] conclusive #EnglishWord  ➕ 2025-09-22 
      "결정적인 (decisive)", "최종적인", **"설득력 있는"*

- [ ] Decisive vs conclusive #EnglishWord ➕ 2025-09-22 
      Decisive -> 의사능력에 초점. 빠르고 효과적으로 결정을 내리는 능력, 행동력과 리더쉽
      conclusive : (conclusion : 결론) -> 결론을 확립하기에 충분한 증거


A 가 그냥 어노테이션을 뜻하고, @A -> 실제로 사용된 어노테이션을 말함.
```java
// 1. 어노테이션 인터페이스 A 정의 (설계도)
@interface MyAnnotation {
    String value();
}

// 2. @A 사용 (실제 적용)
@MyAnnotation("test")  // 이게 @A
public class MyClass { }
```

```java
If the annotation @Documented is present on the declaration of an annotation interface A, 
then any @A annotation on an element is considered part of the element's public contract. 
```
(1) -> 어노테이션 인터페이스 -> 어노테이션 정의하는 것을 얘기함.


```java
(1)
If the annotation @Documented is present on the declaration of an annotation interface A, 
then any @A annotation on an element is considered part of the element's public contract. 

(2)
In more detail, when an annotation interface A is annotated with Documented, 
the presence and value of A annotations are a part of the public contract of the elements A annotates. 

(3)
Conversely, if an annotation interface B is not annotated with Documented, 
the presence and value of B annotations are not part of the public contract of the elements B annotates. 

(4)
Concretely, if an annotation interface is annotated with Documented, 
by default a tool like javadoc will display annotations of that interface in its output 
while annotations of annotation interfaces without Documented will not be displayed
```

(1) 만약 @Documentation 이 어떤 Annotation interface 의 정의에 붙어있으면 (Annotation interface -> 어노테이션 정의부분),
어떤 요소에  어노테이션을 적용한 것은 (@A - Annotation 의 사용) 요소의 public contract 의 부분으로 간주된다.

> Public Contract -> 외부에서 의존할 수 있는 공식 API 부분이라는 뜻.
> -> 다른 개발자가 이 메서드/클래스를 사용할때 반드시 알아야 하는 정보

(2) 더 말하자면, 어떤 어노테이션이 @Documented 와 같이 정의되면
어노테이션 A 의 존재나 값이 A 가 주석을 하는 요소의 public contract 의 부분이 되는 것이다.
-> Annotation 에 관련된 정보가 공식 문서에 들어간다는 얘기로 들린다.

- [ ] Annotate #EnglishWord ➕ 2025-09-22 
      주석을 달다, 해결하다 

(3) 반대로, 어노테이션 B 의 정의가 Documented 어노테이션과 함께 정의되지 않으면,
어노테이션 B 의 값이나 존재 유무는 어노테이션 B 가 annotate 하는 요소의 public contract 의 부분이 되지 않는것

(4) 더 확실히 하자면, 만약 annotation interface 가 Documented 와 함께 정의되면, 
기본적으로 javadoc 같은 도구가 해당 inerface 의 어노테이션을 그 도구의 출력에 보여줄 것이고
그게 아니면 안보여준다.

그러니까 @Documented 와 함께 정의된 annotation interface 는 공식 API 문서의 부분이 된다
-> 공개적으로 보여진다.


## Annotation

Java 언어에서 지원하는 2가지 특수 목적의 참조 타입 (참조 - 객체를 가리키는) 중 하나 (다른 하나는 Enum)

도구나 프레임워크에 의해 특별한 처리가 필요한 메소드에 대해서 명명규칙에 특수한 접두사를 받는게
일종의 관습이었음 (test_someMethod 등등)

Annotation 은 이러한 명명패턴의 단점을 해결하고 대체함.

### 주요 기능

기존의 명명패턴과의 차이.

어노테이션은 사용된 대상의 코드에 영향을 미치진 않는다.
하지만 특정 도구가 해당 요서에 특별한 처리를 할 수 있게 해줌.

명명 패턴은 오타가 발생하면 컴파일 시점에 감지되지 않고
묵시적인 실패를 유발하지만 (그러니까 내 생각에는, 실제로 해당 명명패턴을 읽지 못하고 도구나 프레임워크가
원하는대로 동작하지 않는 것을 의미하는듯.)

애너테이션을 잘못 사용하면 컴파일 에러가 발생.




런타임에 리플렉션 API를 사용해, 특정 애너테이션이 붙은 메서드/클래스를 찾고, 그 정보를 활용할 수 있다.

```java
for (Method m : clazz.getDeclaredMethods()) {
    if (m.isAnnotationPresent(Test.class)) {
        // 테스트 메서드 실행
        m.invoke(null);
    }
}
```

### 어노테이션 타입의 종류 및 선언


- 마커 에너테이션
파라미터를 가지지 않고, 단순히 주석이 달린 요소를 표시만 함. (@Test 등)


- Annotation with Parameter
어노테이션은 파라미터를 가질 수 있음.

> Class Literal -> 클래스 자체에 대한 참조. (String.class)
> Reflection 에서 클래스를 동적으로 다룰때, 제너릭에서 타입토큰을 넘길때, 프레임워크에서 타입정보가 필요할때


- [ ] admittedly ➕ 2025-10-04 #EnglishWord
      솔직히 말하자면, 인정하건대, 사실



### Meta-Annotations

에너테이션 선언시 에너테이션 그 자체에 붙이는 에너테이션. 에너테이션을 '설정'함.
메타 애너테이션은 애너테이션의 "동작 방식"을 설정함.

대표적으로 `@Retention`, `@Target` 이 있다.

- @Retention(RetentionPolicy.RUNTIME)

이 에너테이션이 런타임까지 유지가 되어 리플렉션으로 접근하게 만듦.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnno {
    String value();
}

public class Example {
    @MyAnno("hello")
    public void test() {}
}


import java.lang.reflect.*;

public class Main {
    public static void main(String[] args) throws Exception {
        Method m = Example.class.getMethod("test");
        if (m.isAnnotationPresent(MyAnno.class)) {서
            MyAnno anno = m.getAnnotation(MyAnno.class);
            System.out.println(anno.value()); // "hello" 출력
        }
    }
}
```

만약 Retension Annotation 이 없다면, 컴파일 후 class 파일에 존재하지 않거나
JVM 이 로드할때 버려짐.

- @Target(ElementType.METHOD)
해당 어노테이션이 어떤 요소 (클래스, 메서드 등) 에 쓰일 수 있는지 제한함


@Documented: Javadoc에 포함할지 여부를 지정

@Inherited: 클래스에 붙은 애너테이션이 하위 클래스에도 자동으로 적용될지 지정

@Repeatable: 같은 애너테이션을 여러 번 붙일 수 있게 함


### 질문

#### 어노테이션이 실제로 **어디에** 사용되는가? (예: 클래스, 메서드, 필드 등)

-> 설정 : @Target

- Package declarations
- Type = class, interface, enum and annotation type declarations
- Method declarations (including elements of annotation)
- Constructor declarations
- Type parameter declarations of generic classes, interfaces, method and constructos
- Field declarations
- Formal and exception parameter declarations
- Local variable declarations

#### Predefined Annotations

##### @Target

##### @Retention

SOURCE -> 바이너리 포함 x
ClASS -> 클래스 파일 포함, (.class), 런타임에 JVM 이 로드 X (기본값)
RUNTIME -> 런타임에 reflection API 로 접근

##### @inherited

메타어노테이션 으로, 어떤 어노테이션에 설정하면
그 클래스를 상속받은 클래스에서 해당 어노테이션에 접근 가능.


```java
import java.lang.annotation.*;

@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyAnnotation {}

@MyAnnotation
class Parent {}

class Child extends Parent {}

```

그래서 자식 클래스에서 부모 클래스에 설정된 어노테이션의 값도 접근 가능

##### @Override

해당 메소드가 super type 의메서드를 오버라이드 하는지 컴파일 타임에 검증

##### @SuppressWarnings

특정 경고 억제

##### @Deprecated

해당 요소가 더이상 권장되지 않음
(노후화되서 이제 나중 버전에 제외할 예정)

##### @SafeVarags

^211585

A variable arity parameter with a non-refiable element type can cause hap Pollution
and give rise to compile time unchecked warnings.

###### arity parameter
-> 가변 인자 파라미터.
Arity 는 '개수' 라는 뜻.  `...`(varargs) 로 선언된 파라미터를 variable arity parameter 라 명함.

######  Reifiable
-> 런타임에 타입정보를 완전히 알 수 있는 타입.
non-reifiable -> 런타임에 타입 정보가 사라지는 타입, 즉 generic 을 말함.

###### Type Erasure
Java 의 generic 은 타입 소거 (type erasure) 때문에 런타임에 타입 정보를 알 수 없음.
타입 소거(type erasure)란, 컴파일 시점에 제네릭 타입 정보(예: List\<String>)가 제거되고, 런타임에는 원시 타입(예: List)만 남는 것을 의미.

###### Heap Pollution

arity parameter 가 non-refiable 하면, 내부적으로 Object\[] 로 배열이 만들어지고, 타입정보가 사라짐.
이 배열에 잘못된 타입의 객체가 들어가면, 런타임에 오류가 발생할 수 있음
이를 Heap Pollution 이라 함.

> It is possible that a variable of a parameterized type will refer to an object that is
        not of that parameterized type. This situation is known as heap pollution.

###### unchecked warnings

따라서 non-reifiable arity paramter 를 사용하면 unchecked warning 을 발생시킬 수 있고
이 경고는 타입 안정성이 보장되지 않으니 주의하라는 뜻.

그래서 개발자가 해당 어노테이션을 붙임으로써
메서드 작성자가 해당 메서드가 타입 안전하다고 약속하는 것.
그래서 사용자에게 불필요한 warning 을 보여주지 않게 함.

###### 예시

```java
// UNSAFE - 제네릭 파라미터 배열에 대한 참조를 노출함!
static <T> T[] toArray(T... args) {
    return args;  // 위험!
}

// 위험한 예시
static <T> T[] pickTwo(T a, T b, T c) {
    switch(ThreadLocalRandom.current().nextInt(3)) {
        case 0: return toArray(a, b);
        case 1: return toArray(a, c);
        case 2: return toArray(b, c);
    }
    throw new AssertionError();
}

public static void main(String[] args) {
    String[] attributes = pickTwo("Good", "Fast", "Cheap");
    // ClassCastException 발생!
}

// 안전한 예시
@SafeVarargs
static <T> List<T> flatten(List<? extends T>... lists) {
    List<T> result = new ArrayList<>();
    for (List<? extends T> list : lists) {
        result.addAll(list);
    }
    return result;
}
```

###### 안전한 메서드의 조건

1. varargs 파라미터 배열에 아무것도 저장하지 않는다 (파라미터를 덮어쓰지 않음)

2. 배열(또는 복사본)의 참조가 외부로 노출되지 않는다 (신뢰할 수 없는 코드가 접근할 수 없음)

###### 사용 가능한 위치

static 메서드

final 인스턴스 메서드

private 인스턴스 메서드 (Java 9부터)




#####  @Repeatable

메타 어노테이션.
동일한 어노테이션을 한 프로그램의 요소에 여러번 붙일 수 있게 해줌.
컨테이너 어노테이션이 필요함.


```java
import java.lang.annotation.*;

// 컨테이너 어노테이션
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Schedules {
    Schedule[] value();
}

// 반복 가능한 어노테이션
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Repeatable(Schedules.class)  // 컨테이너 지정
public @interface Schedule {
    String dayOfMonth() default "";
    String dayOfWeek() default "";
    int hour() default 0;
}

// 사용 예
@Schedule(dayOfMonth = "last")
@Schedule(dayOfWeek = "Fri", hour = 23)
public class ScheduledClass {
    // ...
}
```

###### 쓰임새

-> 테스트 프레임워크 : 한 테스트가 여러 예외를 기대할때

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(ExceptionTests.class)
public @interface ExceptionTest {
    Class<? extends Exception> value();
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTests {
    ExceptionTest[] value();
}

// 사용 예시
@ExceptionTest(IndexOutOfBoundsException.class)
@ExceptionTest(NullPointerException.class)
public void testMethod() {
    List<String> list = new ArrayList<>();
    list.get ( 1 ); // IndexOutOfBoundsException
    String s = null;
    s.length(); // NullPointerException
}
```

어떤 테스트 메서드가 여러 종류의 예외를 던질 수 있고, 이 예외들이 모두 "정상 동작"임을 검증하고 싶을 때
예를 들어, testMethod()가 IndexOutOfBoundsException 또는 NullPointerException을 던지면 테스트가 통과해야 한다고 가정.


-> 스케줄링 : 하나의 작업에 여러 스케줄을 지정할때

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Repeatable(Schedules.class)
public @interface Schedule {
    String cron();
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Schedules {
    Schedule[] value();
}

// 사용 예시
@Schedule(cron = "0 0 * * *") // 매시간
@Schedule(cron = "0 0 1 * *") // 매일 1시
public class ScheduledJob {
    // ...
}
```

어떤 작업(Job)이 여러 시간대에 실행되어야 할 때가 있다.
예를 들어, 매시간 한 번, 그리고 매일 새벽 1시에 한 번 실행해야 한다면?


-> 권한 부여 : 하나의 엔드포인트에 여러 권한을 부여할때

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(Permissions.class)
public @interface Permission {
    String role();
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Permissions {
    Permission[] value();
}

// 사용 예시
@Permission(role = "ADMIN")
@Permission(role = "USER")
public void securedEndpoint() {
    // ...
}

```

어떤 API 엔드포인트가 여러 역할(ADMIN, USER 등)에게 모두 허용되어야 할 때.



##### @FunctionalInterface

메타 어노테이션.
-> '함수형 인터페이스' 임을 명시적으로 선언.

###### 함수형 인터페이스 ?

추상 메서드(abstract method) 가 오직 하나만 존재하는 인터페이스.  ([[250927#^733ead]] 참조 )
람다식이나 메서드 참조의 대상이 될 수 있음.

추상 메서드가 2개 이상이면 컴파일 에러 발생.

###### 쓰임새

람다식과 메서드 참조의 대상이 되는 타입을 명확히 하기 위해서.
대표적으로 Runnable, Callable, Comparator\<T>, PredicateT> 등이 함수형 인터페이스.

```java
@FunctionalInterface
interface MyCalculator {
    int calculate(int a, int b);
}

public class Example {
    public static void main(String[] args) {
        // 람다식으로 함수형 인터페이스 구현
        MyCalculator adder = (x, y) -> x + y;
        int result = adder.calculate(3, 5);
        System.out.println(result); // 8
    }
}
```

-> main 메서드에서 람다식 (x, y) -> x + y로 인터페이스를 구현 가능.

###### 예시 : 왜이리 쓰는가?

그냥 method 하나 선언해서 return x + y 하면 되지 않는가 ?


-> 코드 간결성과 유연성

-> API 와 프레임워크의 안정성

-> 동작을 값처럼 전달

-> 자바의 타입 시스템과 안정성
