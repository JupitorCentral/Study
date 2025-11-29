---
tags:
  - Java/Basics
---

## 자바의 자료형

- 기본 자료형 <- -> 참조 자료형

변수 ==초기화== 시에 new 의 사용유무. (예외 : String)

### 기본 자료형 | primitive

#### 정수형

- byte : 1바이트 (8bit)
- short : 2바이트
		byte 로 표현하기엔 너무 수가 크고,  int 로 쓰기엔 값이 너무 작을때 (공간 효율을 위함)
- int : 4바이트
- long : 8 바이트

byte, short, int, long 은 음수 포함 (signed) : (-2^(n bit-1) ~ 2^(n bit -1) - 1)


- char : 2바이트
char 는 음수 없음 : 0 ~ 2^(n bit) - 1

2 바이트 -> unicode
\u 다음에는 16진수 4개가 와야함
그리고 음수가 들어갈 수 없음
그리고 \u로시작하지 않으면 두개 이상의 문자가 대입될 수 없음

#### 소수점 값

- float 4 바이트
- double 4바이트

-> 민감한 숫자 (돈 계산 등) 일때 사용해선 안됨
-> java.math.BigDecimal

> float : single-precision 32-bit IEEE 754 floating point 
> double : double-precision 64-bit IEEE 754 floating point


##### floating point ?

값의 따라 소수점 자리의 위치가 바뀌는 것을 의미.

고정 소수점 : 정수부, 소수부를 나타내는 비트가 정해져 있음.
부동 소수점 : 부호 + 가수 + 지수

예를들어, 123.45 의 경우
고정 소수점 : 123 + 0.45
부동소수점 : $(-1)^0$ + $1.2345$ + $10^2$

##### single, double precision ?

싱글은 그냥 double (64비트) 의 절반이라 싱글이라고 부르는것.

##### IEEE 754 ?

국제표준


#### 나머지

- boolean - true/ false , 대부분의 경우 1바이트


#### 형변환 (Casting)

- 안되는 경우
	- boolean
	- 기본 -> 참조, 참조 -> 기본 (그러나 방법이 없는것은 아님. 단지 Casting 으로는 안될 뿐)

##### bsil fd ?


##### 1의 보수, 2의 보수 


##### & , | , ^

숫자일 경우 : 비트연산
boolean 일 경우 : 논리 연산

^ : xor -> true ^ false = true, true & true | false & false = false

##### && 와 & 의 차이

&& 의 경우, 좌측 항의 결과가 false 면 우측 항을 계산하지 않음. (어차피 false 이기 때문) 
하지만 & 은 좌 우 모두 비교해야 하므로 양쪽 항이 모두 계산됨.


## Primitive Type, reference Type 이 저장되는 공간은 어디인가 ?

자바에는 이미 정의된 primitive 타입이 있다.  로우케이스.
얘네들의 wrapper 클래스는 capital letter로 시작, 그리고 API 에 정의된 Class 이므로 Method 가 있음.

###   Primitive on the stack and heap

==Primitive Type 은 스택 또는 힙에 저장 될 수 있다.== 

local variable, paramter 일 경우 stack 에 저장됨.
member of class 의 경우 (instance variable), instance 가 생성되면서 'heap' 에 저장됨.

-> 그럼 class variable 은 ?
-> metaspace (heap 과 별도로 분리된 공간.)

### Storing object on the heap

#### Reference

reference 는 객체를 가리키고, 우리가 그 객체에 접근할 수 있게 해줌. (여기서 reference 라는 특정 개념이 나오는 듯 하다)
우리가 object instance member (instance variable) 에 접근할때, 우리는 'reference' 를 사용한다.

만약 우리가 static variable 에 접근하면, 우리는 class name 을 사용 (reference 를 사용하지 않는다는 것으로 보임)

reference 는 스택과 힙에 저장.
스택 -> reference 가 local variable 일 경우
heap -> instance variable (variable inside a class) 일 경우 heap

(heap 은 동적 생산이 일때 사용되는 공간이 맞는 것 같다.)

##### reference to others 

abstract class 에 대해 reference 를 가질 수 있음 (abstract class 가 아니라. 근데 애초에 abstract class 가 객체를 가질 수 없잖아)
interface 도 마찬가지.


#### Objects

모든 객체는 'heap' 에 저장된다.
객체를 이해하려면 OOP 의 근간이 되는 이론을 알아야 함.
class 는 집의 설계도. 집이 만들어지면, 그제서야 문을 열고 창문을 닫을 수 있음.

객체가 생성되면, in-memory representation of the class 를 갖게되는 것.
reference 를 이용해서, 우린 instance members 에 접근 할 수 있음 (dot notation syntax)



#### Difference between references and objects

![[Screenshot 2025-09-15 at 12.33.50 AM.png|700]]

```java
class Person {  
    private String name;  
    private int age;  
  
    Person (String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
  
    @Override  
    public String toString() {  
        String decoratedName = "My Name is " + name + " and I am " + age + " years old";  
        return decoratedName;  
    }  
  
    public void setName (String name) {  
        this.name = name;  
    }  
}  
  
public static void main(String[] args) throws Exception {  
    Main mainObj = new Main();  
    int x = 0;  
    Person joeBloggs = mainObj.new Person ("Joe Bloggs", 23);  
    System.out.println(x);  
    System.out.println(joeBloggs.toString());  
    changeName(joeBloggs, "John");
}  
  
public static void changeName (Person person, String newName) {  
    person.setName(newName);  
}
```

이 코드를 실행하는 method 에 대해서, 
첫번째 main () method 가 있고, main () 함수를 위한 frame 을 스택에 쌓게 됨.
그리고 그 프레임안에 main method 의 local variable, parameter 가 생성됨.
이때 new 로 생성된 Person 객체에 대한 reference joeBloggs 도 main 안에 생기게 됨 (local variable 로써)

String Pool 도 Heap 공간에 저장되고, 어떤 새로운 내용을 가진 String Object 가 생기면 거기에 저장해두었다가
나중에 객체를 재사용함. 
그러니까.
```java
String s1 = "My name is Joe Bloggs";        ->> "My name is Joe Bloggs" 라는 String 객체가 String pool 에 생성
String s2 = "My name is Joe Bloggs";         --->> String Pool 에 있던 객체 그대로 씀
```

그러니까 s1, s2 는 reference 이고, "My name is Joe Bloggs" 얘는 "My name is Joe Bloggs" 라는 값을 가지는 String 객체라는 것.
("My name is Joe Bloggs" 같은건 String literal 이라고 한다.)

println method 가 콜 될때, 그를 위한 Frame 이 stack  에 push 됨.
println 실행이 끝나기 전에, joeBloggs.toString() 이 반드시 호출되어야 한다 (-> frame 이 생성됨)

그러니까, 먼저 System.out.println(x);  에 의해서 println 에 대한 frame 생성
그 다음줄에서 toString method 를 위한 프레임 생성 이런 흐름인듯.

이게 헷갈릴 수 있는게, 참 웃긴게 
joeBloggs.toString() 이 실행될때 toString() 에 대한 frame 이 stack 에 쌓이면서
toString() 의 local variable 인 reference decoratedName 이 생성되고, 
String literal 은 Heap 에 생성된다는 것이다.

타이밍은 내가 생각했을땐, toString 에 대한 method 를 위한 frame 이 생성될때 그 메서드 안에 쓰이는 
String literal 들이 String pool 에 생성될 듯.


그래서 class 에 대한 객체가 생성될때 reference 는 객체를 참조하고,
이 reference 가 함수의 parameter 로 넘어갈때 결국 이 reference 자체가 넘어가는 것이 아니라
reference 의 '값' 이 넘어가기 때문에 그리고 이 참조값은 이전에 생성했던 
new Person ("Joe Bloggs", 23);  -> 이 객체이기 때문에
changeName 에서 person 이 new Person ("Joe Bloggs", 23); 객체를 가리키게 되는 것.

결국 참조에 대한 call by value 가 되면서 call by reference 처럼 보이게 되는 것 -> 이거 사람들 사이에 의견이 분분한 것으로 알고 있다.
근데 메모리 관점에서 보면, call by value 가 일단 맞는 듯 하다. 이런 관점에서 보면.

그러니까, changeName executed -> frame for changeName created to stack 
-> person reference created inside that frame
-> call by value, so value related to new Person instance is copied to person local variable (reference) inside that frame.

그러니까 결국 포인터 개념이 맞네. cpp 처럼.

이 이후는 좀 내용이 너무 많아서 pass 하자.

-> 결국 reference 를 잘 다루어야 한다는 얘기.


### BigDecimal in Java

자바에서는 2진수로 저장안하고, 10진수 그대로 저장한다.

```java
// 내부 구조
class BigDecimal {
    BigInteger intVal;    // unscaled value (정수)
    int scale;           // 소수점 자릿수
}

// IEEE 754: 0.1을 이진수 무한소수로 근사
double d = 0.1; // 실제로는 0.1000000000000000055...

// BigDecimal: 정확한 십진수 표현
BigDecimal bd = new BigDecimal("0.1");
// 내부적으로: intVal = 1, scale = 1
// 의미: 1 × 10^(-1) = 0.1

// float/double (근사값 연산)
0.1 (근사) + 0.2 (근사) = 0.30000000000000004 (근사)

// BigDecimal (정확한 십진수 연산)
BigDecimal a = new BigDecimal("0.1");  // intVal=1, scale=1
BigDecimal b = new BigDecimal("0.2");  // intVal=2, scale=1
BigDecimal result = a.add(b);          // intVal=3, scale=1 = 0.3

```

주의할점은 BigDecimal(0.1) 이렇게 쓰면 안되고, BigDecimal ("0.1") 이렇게 
파라미터에 스트링타입을 넣어주어야 된다.

IEEE 754 의 single/double precision 은 표현할 수 있는 정밀도에 한계가 있지만
BigDecimal 의 정밀도는 메모리가 제공되는 한 무한하다. (가변적임)

대신 무한소수는 BigDeicmal 도 피할 수 없는데, 이럴땐 위와 비슷하게 특정 자릿수에서 반올림 & 오차를 사용한다.

```java
import java.math.RoundingMode;

// ✅ 정밀도와 반올림 모드 명시
BigDecimal result = one.divide(three, 10, RoundingMode.HALF_UP);
// 결과: 0.3333333333 (소수점 10자리까지)

// ✅ MathContext 사용
MathContext mc = new MathContext(50, RoundingMode.HALF_UP);
BigDecimal precise = one.divide(three, mc);
// 결과: 50자리 정밀도로 계산
```

즉, 가수 부분을 BigInteger 로 10진수로 표현해서 저장하고 
(실제 내부적으로는 당연히 2진수이지만, 1이상의 자연수는 2진수가 무한대로 표현할 수 있으므로 문제가 안됨)
그다음 지수부분 (scale) 을 따로 저장하는 것.


> [!info] 그래서 BigDecimal 은 아무래도 IEEE 754 보다 느리다.
> IEEE 754 는 하드웨어적으로 직접 연산을 지원하지만 BigDecimal 은 하드웨어적으로 표현하기 때문


####  BigInteger 는 값을 어떻게 저장하는가 ?

```java
// BigInteger 내부 구조 (개념적)
class BigInteger {
    int signum;        // -1(음수), 0(영), 1(양수)  
    int[] mag;         // magnitude (절댓값의 이진 표현)
}

// -123을 BigInteger로 저장
BigInteger negativeNum = new BigInteger("-123");

// 내부적으로:
// signum = -1
// mag = [123의 이진 표현] = [01111011]

BigInteger(int signum, byte[] magnitude) // BigInteger 의 constructor 
```

#### 2의 보수를 안쓰는 이유 ?

```java
// 2의 보수는 고정 크기에서만 의미가 있음
int value = -1;  // 32-bit: 11111111111111111111111111111111

// BigInteger는 임의 크기 지원
BigInteger huge = new BigInteger("-999999999999999999999999999999");
// 2의 보수로는 "몇 비트 기준인가?" 문제 발생
```

2의 보수로는 몇비트 기준인가? 로부터 오는 문제발생에 대해서,
연산에 대해서 생각하면 감이 확온다.

```java
// Sign-Magnitude 방식 (현재 BigInteger)
BigInteger minusOne = new BigInteger("-1");  // signum=-1, mag=[1]
BigInteger huge = new BigInteger("999999999999999999999999");
BigInteger result = minusOne.add(huge);      // 간단한 연산

// 만약 2의 보수를 썼다면:
// -1을 몇 비트로 저장할 것인가?
// 8비트? 16비트? 32비트? 64비트? 80비트?
// huge와 연산하려면 둘 다 같은 길이로 맞춰야 함
// 매번 연산할 때마다 비트 길이 결정 문제 발생
```

만약 2의 보수로 크기가 커질때마다 그 크기를 정하고 1로 채워둘 수 있지만,
위의 경우에는 서로 다른 자릿수 - 크기를 가지기 때문에 연산이 안된다.

-> 그런데 위의 경우, 연산시 동적 확장을 하면 안되는지 ?

이론적으론 가능하다, 실제로 일부 라이브러리에서 그렇게 사용.

그렇지만 성능 오버헤드가 심각함.

Reddit 개발자 경험담:

> _"tried implementing 2's complement for arbitrary precision. The overhead of constantly resizing and sign-extending made it 3-4x slower than sign-magnitude for most operations"_



#### 문서에서 Two's complement 처럼 동작한다고 말하는 것에 대한 의미

```java
// 비트 연산은 2의 보수처럼 동작
BigInteger a = new BigInteger("-5");
BigInteger result = a.shiftRight(1);  // 부호 확장 수행
System.out.println(result);  // -3 (2의 보수 규칙 따름)
```
