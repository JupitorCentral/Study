---
tags:
  - Java/OOP
---

## Abstract

```java
abstract class MemberManagerAbstract {
        int num;

    public abstract boolean addMember();
    public void printLog(String data) {
        System.out.println("Data="+data);
    }
}
```

- abstract 개수
- static, final
- How to use at other class
- inheritence

-> partial interface


## final

-> to class, variable, method

### class

- inheritence

### method


```java
public class SubMain {
    private final void println (String obj) {
        System.out.println(obj);
    }
    private final void println (int value) {              ->>> 이건 가능
        System.out.println(value);
    }
}
```

- overriding

### variable

- variable <---> method

- local variable
- paramter


```java
public class SubMain {
    private final void println (String obj) {
            final int finalValue;
                finalValue = 3;
                finalValue = 5;                                   ->>> ERROR

        System.out.println(obj);
    }
    private final void println (final short value) {
        System.out.println(value);
    }
    private final void println (final int value) {
        value = 3;                                                     ->>> ERROR
        System.out.println(value);
    }
}
```

- DTO and its instance variables


## enum

### basic
```java
public enum OverTimeValues {
    THREE_HOUR,
    FIVE_HOUR,
    WEEKEND_FOUR_HOUR,
    WEEKEND_EIGHT_HOUR
}

public static void main(String[] args) throws Exception {
    OverTimeValues enum1 = OverTimeValues.FIVE_HOUR;
    println(enum1);
}

public static <T> void println (T obj) {
    System.out.println(obj);
}

---

> Task :Main.main()
FIVE_HOUR

```

- constructor ?
- result and 'String' ?


### with certaim value
```java
public enum OverTimeValues {
    THREE_HOUR(18000),
    FIVE_HOUR(30000),
    WEEKEND_FOUR_HOUR(40000),
    WEEKEND_EIGHT_HOUR(60000);

    private final int amount;
    OverTimeValues(int value) {
        this.amount = value;
    }
    public int getAmount() {
        return this.amount;
    }
}

public static void main(String[] args) throws Exception {
    OverTimeValues enum1 = OverTimeValues.FIVE_HOUR;
    println(enum1.amount);
}

----

> Task :Main.main()
30000
```

-> access modifier for constructor ?



### parent of enum

#### constructor of Enum
  -> name, ordinal

#### 'overriding' blocked methods inherited from Object at Enum Class
- availabe methods


#### method declared at Enum
- compareTo(E e)
        - bigger -> later
```java
public static void main(String[] args) throws Exception {

        OverTimeValues o1 = OverTimeValues.THREE_HOUR;
        OverTimeValues o2 = OverTimeValues.FIVE_HOUR;

        println(o1.compareTo(o2));
}

-----
> Task :Main.main()
-1
```

- getDeclaringClass()
- name()
- ordinal()
- valueOf(Class\<T> enumType, String name)
- values()


## Nested Class


##### static
- inner class <- -> static nested class

##### nameless
- inner class -> local inner class, anonymous inner class

### nested class 의 사용이유

#### logical

한 곳에서만 쓰이는 클래스를 논리적으로 묶어야 할때

#### encapsulation

어떤 한 클래스의 private 변수에 접근하는 클래스 B 가 있고, 이 클래스를 외부에 노출시키고 싶지 않을때

#### readability

가독성과 유지보수성을 위해


### static nested class

##### access range of inner class
외부 클래스의 어떤 변수도 접근 가능 (private 까지)
-> static 은 불가능 ㅋㅋ (뭐, instance 화가 안되어있으니 해당 객체가 메모리에 할당이 안되어 있을테니 말이다)


##### compile ...?
```java
public class Main {

    static class innerStaticClass {
        private int value;
        public int getValue() {
            return this.value;
        }
        public void setValue(int value) {
            this.value = value;
        }
    }
    public static void main(String[] args) throws Exception {
    }
}
------------------

-> 이러면 컴파일 후에 Main.class, Main$StaticNested.class 파일이 생성된다!
```

##### how to create a instance of static nested class

```java
Main.StaticInnerClass staticInnerClass = new Main.StaticInnerClass();
---> 근데 "Main." 안써도 되던데 ,,,? 자바 버전이 달라서 그런가 ?
```


### inner class & anonymous class


#### inner class
##### instantiate

```java
Main.InnerClass innerClass = new Main.InnerClass();
-->  ㅋㅋ 이거 안된다. static 이 아니면, Main 의객체가 생성되지 않는한 InnerClass 의 생성자의 함수가 메모리에 할당되지 않기 때문

Main mainObj = new Main();
Main.InnerClass innerClass = mainObj.new InnerClass();
----> 이래야 가능
```

##### GUI

내부 클래스는 외부에서 쓸일이 전혀 없는 경우에 씀.
주로 GUI 관련 프로그램을 개발할때 쓴다.


#### Anonymous class

```java
public class Main {

    public interface EvenListener {
        public void onClick();
    }

    class MagicButton {
        private EvenListener listener;

        public void setListener(EvenListener listener) {
            this.listener = listener;
        }
        public void onClickProcess () {
            if(this.listener != null) {
                this.listener.onClick();
            }
        }
    }

    class showMyNameClickListener implements EvenListener {
        @Override
        public void onClick() {
            println("This is my Name : Eric");
        }
    }

    public static void main(String[] args) throws Exception {
        Main mainObj = new Main();
        MagicButton magicButton = mainObj.new MagicButton();
        magicButton.setListener(mainObj.new showMyNameClickListener());
        magicButton.onClickProcess();

        magicButton.setListener(new EvenListener() {
            @Override
            public void onClick() {
                println("This is my Name : John");
            }
        });
        magicButton.onClickProcess();
    }
}
```

Anonymous 클래스를 쓰면, 한번만 쓸 것 같은 클래스에 대해서는 이렇게 즉석으로 만들 수 있음.
그리고 클래스를 저장하는데에 메모리도 안쓰게 되고.
(보통 클래스를 정의하면, 정의자체가 메모리 용량을 쓰는 것이기 때문에.)


### Characteristics of Nested class

##### Which variable static inner class can access among parent class's variables

-> 당연히 static 하게 정의된 것만 참조 가능.

```java
static int privateInt = 5;
int normalInt = 3;

static class StaticInnerClass {
    int value;
    public static void showValue (int value) {
        println(value);
    }
}

public static void main(String[] args) throws Exception {
    Main mainObj = new Main();

    StaticInnerClass.showValue(privateInt);
    StaticInnerClass.showValue(normalInt);    --->>> "ERROR"
    StaticInnerClass.showValue(mainObj.normalInt);      ---->> 대신 이건  "ERROR" 가 아니다. 왜냐하면, normalInt 에 대한 메모리가 mainObj 가 할당되면서 할당되었기 떄문.
}
```


##### vice versa

당연히 외부 클래스로도 내부 클래스의 변수 참조 가능


#### Enumeration Class

모든 enum type 은 abstract class `Enum` 의 서브클래스들이다. 
그래서 Enum 에 정의된 메소드들을 사용가능.

그래서 만약에
```java
public enum Size {  
    SMALL ("S"),  
    MEDIUM,  
    LARGE,  
    EXTRA_LARGE  
}

public enum Size {  
    SMALL ("S"),  
    MEDIUM,  
    LARGE,  
    EXTRA_LARGE;  
  
    private final String sizeName;  
  
    Size(String sizeString) {  
        this.sizeName = sizeString;  
    }  
    Size() {  
        this.sizeName = "None";  
    }  
}
```

이렇게 쓰면 SMALL ("S") 때문에 컴파일이 불가능한데,
SMALL ("S") 라는 것은 기본 컨스트럭터를 사용하지 않고
파라미터가 있는 컨스트럭터를 사용하겠다는 의미이기 때문이다.

여기에는 parameter 가 있는 constructor 가 정의되어 있지 않기때문에 에러 발생.

그래서 아래 것이 허용되고, 이때 parameter 가 없는 constructor 를 지우면 MEDIUM, LARGE, EXTRA_LARGE 에 컴파일 오류가 뜬다.

그래서
```java
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
```

이것에 대한 의미는, 뭐지? ㅋㅋ

이전 예시의 EXTRA_LARGE 는 Size 의 subclass 의 객체라고 하던데...