
####  [[Spaces/Study/F-Lab/1주차 공부/250906.md#^a7bfed|synchornize]]

->  [[Spaces/Study/F-Lab/1주차 공부/250907.md#^65dc01|answer]]


####  variable++ / variable-- 와, ++variable / --variable 의 차이

##### 내가 아는 정보
 후위증감연산자 -> 해당 statement 가 끝나고 나면 해당 variable 의 값이 증가.
즉, 계산시 값이 증가하기 전의 값이 계산된 후에야 값이 증가된다.

전위 증감 연산자는 그 반대로, 해당 statement 가 끝나기 전 값이 증가한 후에 계산이 들어감.

##### 정답
statement 실행시, 값이 변하는 시점은 같다. 
그러나, 값이 변하기 전 값을 넘겨주느냐, 값이 변한 후의 값을 넘겨주냐의 차이 
(which value is returned at the expression)


#### 상속과 class variable

[[Spaces/Study/F-Lab/1주차 공부/250908.md#^6801f4|problem]]

이게 뭔소리지 ?

A 클래스가 있고 A 클래스를 상속받은 B 클래스가 있다.
A 클래스에는 static 변수 a 가 있다.
이때 B 클래스와 A 클래스가 메모리에 로딩될때, 변수 a 를 공유하는가 ?

```java
class ParentClass {  
    static int staticVariable = 5;  
  
    public static int getStaticVariable() {  
        return staticVariable;  
    }  
  
    public static void setStaticVariable(int staticVariable) {  
        ParentClass.staticVariable = staticVariable;  
    }  
}  
  
class ChildClass extends ParentClass {  
    public static int getStaticVariable() {  
        return staticVariable;  
    }  
  
    public static void setStaticVariable(int staticVariable) {  
        ParentClass.staticVariable = staticVariable;  
    }  
}  
  
public static void main(String[] args) throws Exception {  
    println("staticVariable from Parent : " + ParentClass.getStaticVariable());  
  
    ChildClass.setStaticVariable(6);  
    println("staticVariable from Child : " + ChildClass.getStaticVariable());  
    println("staticVariable from Parent : " + ParentClass.getStaticVariable());  
  
}
-----------
> Task :Main.main()
staticVariable from Parent : 5
staticVariable from Child : 6
staticVariable from Parent : 6
```

참고로 static variable, method 를 클래스의 static method 에서 호출할때
```java
this.staticVariable;
```
이런식으로 호출이 안되는데, 이는 'this' 가 instance reference 이기 때문이다.
-> 즉 객체가 생성되어야 사용될 수 있는 키워드가 this

그래서 ChildClass 에서도 this.staticVariable 이 안된다. 



#### 객체에 대해 == 은 같은 '주소' 를 의미하는지. 

[[Spaces/Study/F-Lab/1주차 공부/250909.md#^d70982|constant pool]]

변수 명이 다른데, 과연 같은 객체라고 할 수 있는지.
String s1 -> 여기서 s1 이라는 변수는 포인터라고 할 수 있는 것이 아닌지.
확인이 필요함. 


#### 왜 StringBuffer 는 Thread safe 하고, StringBuilder 는 그렇지 않은가 ?




#### return type 이 interface 인데 출력하는 경우

[[Spaces/Study/F-Lab/1주차 공부/250909.md#^3021fc|problem]]



#### bsil fd



#### 2의 보수, 1의 보수




# Questions my AI summarizing

오늘 내용에 맞춰진 질문들

> **자바의 모니터 락은 어떻게 작동하며, 스레드와의 관계는 무엇인가?**  
>    •  모니터 락이 스레드 안전성을 어떻게 보장하는가?  
>    •  모니터 락과 세마포어의 차이점은 무엇인가?  
>    •  스레드 간 경쟁 조건을 피하기 위해 모니터 락이 필수적인 이유는?  
>    •  모니터 락의 성능 영향을 최소화하는 방법은?  
>    •  싱크로나이즈 블록 내에서의 모니터 락 동작 방식을 코드 수준에서 설명하라.

> **어레이 리스트에 다수의 스레드가 동시에 접근할 때 어떠한 문제가 발생할 수 있는가?**  
>    •  멀티 스레드 환경에서의 ArrayList는 어떻게 동작하는가?  
>    •  ArrayList의 스레드 안전성을 높이기 위한 전략은?  
>    •  동기화가 ArrayList의 성능에 끼치는 영향은?  
>    •  ArrayList 대신 사용할 수 있는 스레드 안전한 컬렉션 클래스는?  
>    •  synchronizedList와 ArrayList의 차이점 및 활용 방법은?

> **부동 소수점 연산 시 발생하는 오류는 무엇이며, 이를 어떻게 해결할 수 있는가?**  
>    •  부동 소수점의 근본적인 한계는 무엇인가?  
>    •  플로팅 포인트 아키텍처의 구조를 설명하라.  
>    •  정밀한 계산이 요구되는 시스템에서 부동 소수점 대신 사용할 수 있는 대안은?  
>    •  부동 소수점 연산을 정확하게 테스트하기 위한 방법은?  
>    •  IEEE 754 표준이 부동 소수점 연산에 끼친 영향은?

> **이해 보수를 사용하는 이유 및 그 작동 원리는 무엇인가?**  
>    •  이해 보수와 일의 보수의 근본적인 차이점은?  
>    •  컴퓨터가 이해 보수를 통해 음수를 표현하는 이유는?  
>    •  이해 보수가 계산에서 제공하는 장점은 무엇인가?  
>    •  이해 보수의 활용 사례를 예를 들어 설명하라.  
>    •  컴퓨터 아키텍처에서 이해 보수 연산이 중요한 이유는 무엇인가?

> **자바의 해시 코드와 관련한 해시맵의 내부 구현 원리는?**  
>    •  햏코드 충돌을 해결하기 위한 JDK의 해시맵 전략은?  
>    •  해시맵의 성능 최적화를 위한 데이터 구조는 무엇인가?  
>    •  해시맵에 영향을 끼치는 해시 함수의 설계 전략은?  
>    •  해시맵 내부에서 해시 코드가 어떻게 사용되는가?  
>    •  JDK에서 해시맵과 관련된 성능 이슈를 해결한 사례는 무엇인가?