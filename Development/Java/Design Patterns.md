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

## Strategy Pattern

Strategy Pattern은 런타임(runtime)에 알고리즘(algorithm)을 선택할 수 있게 해주는 behavioral 디자인 패턴

게임을 예시로 든다면, 캐릭터의 이동 방식에는 걷기 - 달리기 - 하늘을 날기 등이 있을 수 있음.

주요 구성 요소
Strategy Pattern의 3가지 핵심 컴포넌트는:​
	Context - 전략을 사용하는 객체. 전략 객체에 대한 참조를 유지하고 있음
	Strategy Interface - 모든 전략이 구현해야 하는 인터페이스
	Concrete Strategies - Strategy Interface를 구현하는 실제 전략 클래스들


```java
// Strategy Interface
interface OfferStrategy {
    double getDiscountPercentage();
}

// Concrete Strategy 1
class NoDiscountStrategy implements OfferStrategy {
    public double getDiscountPercentage() {
        return 0;
    }
}

// Concrete Strategy 2  
class QuarterDiscountStrategy implements OfferStrategy {
    public double getDiscountPercentage() {
        return 0.25;
    }
}

// Context
class StrategyContext {  
    private double price;  
  
    public StrategyContext(double price) {  
	       this.price = price;  
    }  
  
    public void applyStrategy(OfferStrategy strategy) {  
	       double finalPrice = price - (price * strategy.getDiscountPercentage());  
	       System.out.println("Price after offer: " + finalPrice);  
    }  
}

// Client
public class StrategyClient {
    public static void main(String[] args) {
        StrategyContext context = new StrategyContext(100.0);

        // 1) 아무 할인 없음
        OfferStrategy noDiscount = new NoDiscountStrategy();
        context.applyStrategy(noDiscount);

        // 2) 25% 할인
        OfferStrategy quarterDiscount = new QuarterDiscountStrategy();
        context.applyStrategy(quarterDiscount);

        // 3) 런타임 조건에 따라 선택
        boolean isVip = true; // 예: DB, 요청 값 등에서 결정
        OfferStrategy strategy = isVip ? new QuarterDiscountStrategy() : new NoDiscountStrategy();
        context.applyStrategy(strategy);
    }
}
```

