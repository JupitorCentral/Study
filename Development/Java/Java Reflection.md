---
tags:
  - Java/Reflection
---

### Java Reflection Library

```java
// 1. 기본 진입점
java.lang.Class          // 모든 리플렉션의 시작점

// 2. java.lang.reflect 패키지의 주요 클래스들
java.lang.reflect.Method      // 메서드 정보/실행
java.lang.reflect.Field       // 필드 정보/접근
java.lang.reflect.Constructor // 생성자 정보/실행
java.lang.reflect.Modifier    // 접근제어자 확인
```


클래스가 뭘할 수 있는지 분석할 수 있다면 그것을 `reflective` 하다고 말한다.

`Java Reflection` 을 이용해
1. 런타임에 클래스가 뭘할 수 있는지 파악
2. 객체를 런타임에 면밀히 관찰 (inspect) 할 수 있음
	-> 예를들면, 모든 클래스적용되는 toString method 를 만든다던지
3. Generic array 조작 코드를 구현할 수 있음 (뭔소리야)
4. C++ 에서와 같은 Function Parameter 같이 동작하는 `Method` 객체의 이점을 누릴 수 있다.


#### `Class` class

class 명이 Class 이다. `class` 는 키워드, 여기서 Class 는 클래스명.

프로그램이 실행될 동안 자바 런타임은 모든 객체에 대해 `runtime type idenfication` 이라는 것을 유지한다.
이는 JVM 이 올바른 method 를 선택하는데에 쓰인다.

우리도 `Class` 라는 클래스를 이용해 이 정보에 접근 가능.

```java
Employee e;

(1)
Class cl = e.getClass();

(2)
“String className = "java.util.Scanner";
Class cl = Class.forName(className);”

(3)
Class cl1 = Scanner.class; // if you import java.util.*;
Class cl2 = int.class;
Class cl3 = Double[].class; 
```

클래스의 getName 으로 패키지 + 클래스명 을 String 으로 반환받을 수 있음.

> [!info] 사실 `Class` 클래스는 Generic 클래스이다.
> getName 은 배열에 대해선 이상하게 출력된다. 역사적 이유때문에...

```java
public enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE; }

Size.SMALL.getClass() == Size.class // true

-------------------

public enum Size
{
   SMALL,
   MEDIUM,
   LARGE,
   EXTRA_LARGE
   {
      public String toString() { return "XL"; }
   };
}

Size.EXTRA_LARGE.getClass() == Size.class // false
```

2번째 예시는 뭘 의미하는거지 ?
