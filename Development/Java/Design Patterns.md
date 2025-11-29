---
tags:
  - Java/DesignPattern
---

## Singleton Pattern

오직 하나의 인스턴스만 생성하도록 보장하는 패턴.

### 구현 방식

#### 1. Eager Initialization (Private Static Final)
- 클래스 로딩 시점에 인스턴스 생성.
- Thread-safe 하지만, 사용하지 않아도 메모리 점유.

```java
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() { return INSTANCE; }
}
```

#### 2. Lazy Initialization (Synchronized)
- 필요할 때 생성. 동기화 비용 발생.
- Double Checked Locking 등으로 최적화 가능.

#### 3. Enum Singleton (권장)
- 가장 안전하고 간편한 방법.
- 직렬화/역직렬화 시에도 싱글톤 보장. Reflection 공격 방어.

```java
public enum Singleton {
    INSTANCE;
    public void doSomething() { ... }
}
```

## Adapter Pattern

호환되지 않는 인터페이스를 가진 클래스들을 함께 동작하게 해주는 패턴.
- 클라이언트가 기대하는 인터페이스를 구현한 Adapter 클래스를 만듦.
- Adapter 클래스는 내부적으로 실제 객체(Adaptee)를 감싸서(wrap) 호출을 위임.
