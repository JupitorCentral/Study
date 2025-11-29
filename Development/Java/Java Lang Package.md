---
tags:
  - Java/Lang
---
## java.lang package

import 를 안해도 사용가능!

### 가끔 나타나는 에러


#### OutOfMemoryError (OOME)

-> 잘못 프로그래밍되어, 메모리가 부족한 경우

#### StackOverflowError

-> stack 에는 ==어떤 메소드가 어떤 메소드를 호출했는지== 에 대한 정보를 관리함.
-> 이를 통해, 메소드의 깊이가 너무 깊으면 발생하는 에러.


### Classes related to Number

-> ==기본 자료형 (primitive Type) 은 Heap 이 아니라, stack 에 저장된다==
-> 따라서, 계산시 빠른 처리가 가능.

-> Byte, Short, Integer ... (앞글자가 대문자)
-> 겉으로 보기에 참조자료형이지만, 컴파일러에서 자동으로 primitive type으로 형변환 해준다 -> 기본자료형 처럼 쓸 수 있다.
(그래서 앞전에 call by value 가 되었던 것.)


#### wrapper class
-> Character , Boolean 을 제외한 숫자를 처리하는 클래스들은 감싼 클래스라고 불림
-> Number 라는 abstract class 를 확장함.

-> 값이 너무 커서 보기 힘들면
```java
Integer.toBinaryString(Integer.MAX_VALUE);
Integer.toHexString(Integer.MAX_VALUE);
```