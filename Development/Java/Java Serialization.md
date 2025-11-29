---
tags:
  - Java/Serialization
---

## Serializable

마커 인터페이스

객체를 바이트 스트림으로 변환하거나, 복원하기 위핸 인터페이스
메서드나 필드가 없음
선언시 적용하면, JVM 이 해당 클래스를 스트림으로 변환할 수 있게 됨
(이 클래스는 직렬화 가능하다 라는 선언, 신호)

### 직렬화 (Serialization) 이란 ?


데이터를 스트림으로 표현하는 것.

이진 스트림 -> binary code 형태로 데이터를 직렬화
텍스트 스트림 -> 사람이 읽을 수 있는 캐릭터로 데이터를 직렬화

#### 예시

```java
import java.io.*;
class HelloWorld implements Serializable {
    String msg;
    HelloWorld(String msg) { this.msg = msg; }
}

// 직렬화 (이진 스트림)
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("hello.ser"))) {
    oos.writeObject(new HelloWorld("Hello world"));
}

// 텍스트 스트림 저장
try (FileWriter fw = new FileWriter("hello.txt")) {
    fw.write("Hello world");
}
```

이게 변환되면

- **이진 스트림(hello.ser, 16진수로):**

    `AC ED 00 05 73 72 ...`
    (이 값은 자바 직렬화 포맷의 매직넘버와 클래스 정보, 필드 값 등이 포함됨)

- **텍스트 스트림(hello.txt, 16진수로):**

    `48 65 6C 6C 6F 20 77 6F 72 6C`
    (각각 'H', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l'의 ASCII 코드)


#### 이진 스트림으로 데이터를 직렬화 하는 이유 ?

- 이진 스트림(직렬화)은 객체의 모든 정보를 바이트 단위로 저장하므로, 컴퓨터가 바로 읽고 쓸 수 있어 효율적이고 빠르다.

- 텍스트 스트림(예: JSON, XML)은 사람이 읽기 쉽지만, 변환 과정에서 타입 정보가 손실되거나, 구조가 복잡해질 수 있다.


#### 이진 스트림의 단점, 위험
from effective java

1. 보안 취약점


2. 유지보수 및 호환성 문제


3. 객체 불변성/불변 조건 위반 (Invariant Corruption)


4. 성능 및 메모리 문제


5. 정보 은닉 실패



#### 왜 객체를 그대로 전송할 수 없는가 ?

```java
String str = "Hello World";
```

여기서 str 은 참조고, "Hello World" 는 Heap 에 존재하는 String Pool 에 있는 Literal String 이라는 객체이다.
그리고 이 객체는 물리적으로 binary code 로 존재할 것이다.
왜 이 객체를 그대로 전송할 수 없는가 ?

힙에 저장된 객체의 데이터는 JVM 의 메모리 구조에 맞게 저장되어 있음.
이 구조는 JVM 마다, OS 마다 다르다.

힙에 있는 객체의 바이너리 데이터를 그대로 복사해서 전송한다 하더라도,
다른 환경에서는 해석이 불가.

또한 객체 내부에 다른 객체를 참조할 경우 그 참조값은 무의미함.

즉 객체는 메모리 주소 기반이기때문에 그대로 보낼 수 없음.

따라서 '직렬화' 란,
객체의 상태(필드 값 등)을 JVM 내부 메모리 구조(실제 메모리 주소, 힙 레이아웃 등)와 무관한
표준화된 바이트 스트림으로 변환하는 것을 의미함.

JVM 메모리 구조는 그때그때마다 다르다. 주소도 당연히 다르고.


그래서 Oracle 에서는 JVM 간 소통할 수 있는
'Object Serialization Stream Protocol' 을 정의해두었다.
(https://docs.oracle.com/javase/8/docs/platform/serialization/spec/protocol.html)

##### Object Graph

자바의 메모리 구조와 무관하게 객체를 전송할떄
객체가 다른 객체와 연결되어있는 경우 (다른 객체를 해당 객체의 field 에서 참조할 경우)
이를 기존 JVM 의 메모리 구조와 무관하게 보내기 위해
다른 객체에 대한 참조를 일련번호 (serial number) 로 변환하여 보냄.

한 객체에서 시작해서 그 객체가 참조하는 모든 객체들(그리고 그 객체들이 또 참조하는 객체들...)의 연결 구조 전체를 의미.


### 왜 직렬화 기능을 제공할까 ?

Once an object has been serialized, its
encoding can be sent from one VM to another or stored on disk for later deserialization.

-> 즉 객체의 보존을 위해 파일에 저장되거나, 다른 서버 에 객체의 정보를 전송하기 위해 직렬화 함.

-> 네트워크 통신을 위해서 필요함.

-> 직렬화와 역직렬화를 사용하면 객체의 깊은 복사를 통해 객체 그래프 전체를 복제 가능.

-> 객체를 직렬화하여 디스크, 메모리에 저장해두고 필요할때 빠르게 복원 가능
        -> 파일에 영구저장
        -> 캐싱 (redis)




### Serializable 을 구현하면 어떤 장단점이 있는가 ?

여기서 Serializable 을 구현한다는 것은, Serializable interface 를 implements 한다는 것을 의미함.


### Marker Annotation 에 비해 Marker interface 가 가지는 장점

Marker interface 패턴을 사용하는데, 이는 메서드는 없지만
그 인터페이스를 implement 하는 클래스가 해당 속성을 가짐을 의미함.

그리고 이렇게 함으로써 해당 클래스의 객체가 `ObjectOutputStream` 에 쓰여질 수 있음을 나타냄.

Serializable을 구현한 클래스는 "Serializable 타입"으로 간주.
따라서, 메서드의 매개변수 타입으로 Serializable을 지정하면,
컴파일러가 "이 메서드에 전달된 객체가 Serializable인지"를 컴파일 타임에 검사할 수 있음.


```java
public void saveObject(Serializable obj) {
    // obj는 반드시 Serializable을 구현한 객체여야 함
    // 직렬화 관련 로직 작성 가능
}

String str = "Hello"; // String은 Serializable 구현
saveObject(str); // OK

Object obj = new Object(); // Object는 Serializable 구현 X
saveObject(obj); // 컴파일 에러!
```

어노테이션도 컴파일 타임에 에러를 낼 수 있으나
추가 도구가 필요함.

인터페이스는 자바 자체의 타입 시스템의 이점을 누릴 수 있음.


### 직렬화 대상 클래스 설계시, 어떤 점을 고려해야 하는가 ?

1. 직렬화가 반드시 필요한가 ?

Serialization is dangerous and should be avoided.
If you are designing a system from scratch,
use a cross-platform structured-data representation such as JSON or protobuf instead.

2. 직렬화 형태와 호환성

직렬화 대상 클래스의 내부 구조(필드 등)는 외부 API가 됨.
한 번 배포하면, 그 형태를 영원히 유지해야 하므로 신중하게 설계해야 함.


3. 불변식 (객체의 일관성) 과 보안

- readObject는 숨겨진 생성자 역할을 하므로, 객체의 불변식(예: 필수 필드 null 불가 등)을 반드시 검증해야 함.
- mutable 객체가 있다면 방어적 복사(defensive copy)도 필요함.

흠 3가지 다 잘 모르겠다.


### 역직렬화시 발생할 수 있는 보안이슈? 방지 방법 ?

#### 보안 이슈

- Remote Code Execution (RCE)

조작된 바이트 스트림이 역직렬화될때 특정 코드가 실행될 수 있음.

- Denial of Service

지나치게 복잡한 object graph (deserialization bomb) 등을 보내 시스템 자원을 고갈시킬 수 있음.

- 내부 상태 변조 및 class invariant violation

Class Invariant - 클래스 불변식(Class Invariant)은 객체의 상태가 항상 만족해야 하는 조건 또는 규칙을 의미.
즉, 객체가 생성된 이후부터 소멸될 때까지 특정 속성이나 관계가 항상 참이어야 한다는 것.

예를 들어, `Rational` 클래스에서 분모(denominator)는 0이 될 수 없다는 규칙이 있다면,
"denominator는 0이 아니다"가 그 클래스의 불변식(invariant)이 됨.

- Gadget Chain

신뢰할 수 없는 클래스들의 메서드가 연쇄적으로 호출되어 공격자가 의도한 행위를 수행

#### 방지 방법

- **신뢰할 수 없는 데이터 역직렬화 금지**
- **직렬화 필터(ObjectInputFilter) 사용**
- **직렬화 프록시 패턴(Serialization Proxy Pattern) 적용**
- **readObject 메서드에서 방어적 복사 및 유효성 검사 수행**
- **serialVersionUID 명시적 선언**
- **가능하면 Java 직렬화 대신 JSON, Protocol Buffers 같은 대체 기술 사용**

### serialVersionUID


-> 직렬화된 객체의 클래스 버전을 식별하는 고유 값.

직렬화/역직렬화시, 송수신 서버가 같은 serialVersionUID 를 가지면 동일 클래스로 간주.


동일 클래스 명이어도 서로 다른 프로젝트에서 선언된 변수, 메소드가 다를 수 있음.
또 같은 클래스여도 일부 변수/메소드가 추가되거나 수정될 수 있음.

이걸 방지하기 위해서 사용하는 것.
