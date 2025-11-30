---
tags:
  - Java/Basics
---

## 연산자

-> 참조 자료형은 +, -, /, * 사용 불가 
-> 예외, String 은 + 사용 가능

#### 논리 부정 연산자 (logical complement operator)
-> only boolean

#### 배타 연산자 (xor)
^

##### 그리고 비트 반전 연산자가 있었던 거 같은데... 
-> '~' : 틸드 



#### 계산하는 순서

단항 > 산술 (곱하기나누기 > 더하기 빼기)

우선순위가 높은 순위대로 오른쪽에서부터 왼쪽으로

##### 근데 ++, -- (마이너스x2)  가 어디에 위치하느냐에 따라서 다르다. 


#### 논리 연산자

&& -> AND
|| -> OR


#### 삼항 연산자 : ?

expression ? if expression is true : if expression is false

instanceof 도 연산자. -> 10장에서.
비트 연산을 하는 7개의 연산자.

## 가변 parameter (varargs)

```java
accessModifier returnType methodName(datatype... variableName) {
    // method body
}
```

```java
public class VarargExample {
    public int sumNumber(int... args) {
        System.out.println("argument length: " + args.length);
        
        int sum = 0;
        for(int x : args) {
            sum += x;
        }
        return sum;
    }
    
    public static void main(String[] args) {
        VarargExample ex = new VarargExample();
        
        // 다양한 개수의 인자로 호출 가능
        ex.sumNumber();              // 0개
        ex.sumNumber(10);            // 1개
        ex.sumNumber(10, 20);        // 2개
        ex.sumNumber(10, 20, 30, 40, 50); // 5개
    }
}
```

### 내부 동작 원리

varargs는 내부적으로 배열로 처리된다. 즉, `int... nums`는 실제로 `int[] nums`로 컴파일되며, 메서드 내부에서는 배열 문법을 사용해 접근한다. 
인자가 없으면 length가 0인 배열이 생성된다.

### 주요 제약 사항

-  하나의 메서드에 하나만 사용 가능
-  varargs는 **반드시 파라미터의 마지막에 위치**해야 한다.

