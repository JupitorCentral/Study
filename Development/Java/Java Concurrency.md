---
tags:
  - Java/Concurrency
  - CS/Thread
---

## Thread Safety & Visibility

### 클래스가 ThreadSafe 하다는게 무슨 의미인가 ?

여러개의 쓰레드들이 구동되는 프로그램들 중에서, 
쓰레드들 사이에서 원하지 않는 동작이 발생하지 않는 것.

thread-safty 한 클래스와 그렇지 않은 클래스를 구분하는 가장 큰 특성
-> correctness. 

#### Correctness 란 무엇인가 ?

클래스가 그에 대한 요구사항 (specification) 을 잘 따르는 것을 말함.
객체의 상태를 저장하는 변수를 정의하고, 어떤 동작 (method) 에 의해 변하는 상태를 잘 묘사할 수 있는 specification.

즉, **Thread safe 한 클래스**는:
여러개의 쓰레드에 의해 접근이 되어도, 객체의 correctness 를 잘 유지하며 동작하는 객체의 클래스.

##### ThreadSafe 한 클래스의 예시 - A Stateless servlet
-> 상태가 없는 클래스는, Thread-safe 하다.
stateless 한 클래스이다 -> 그 클래스에는 내부에 field가 없고, 다른 클래스의 field 를 reference 하는게 없음을 의미.
즉 stateless 한 객체는 항상 Thread-Safe 하다.

### Atomicity

만약 클래스의 field 가 하나라도 생기면 어떻게 될까?
`++count;` 이 단순한 코드는 3가지 동작(Read-Modify-Write)으로 나뉜다.

1. 해당 변수(상태)에 현재 값을 가져온다
2. 가져온 값에 1을 더한다
3. 그리고 해당 변수에 구한 값을 더한다.

이 과정 사이에 다른 스레드가 개입하면 문제가 발생한다. 이를 **Race Condition**이라 한다.

#### Race condition
여러 쓰레드가 동일한 상태에 접근하는 경우 접근 순서에 따라 값이 바뀌는 상황.
Race condition을 해결하기 위해선, 우리가 어떤 상태를 변경하고 있는 도중에 다른 Thread가 그 값을 사용하는 것을 막아야 한다.

### Memory Visibility
한 쓰레드가 변경한 값을 다른 쓰레드가 볼 수 있는 특성.
`Synchronized`는 단순히 Atomicity(상호 배제)뿐만 아니라 Memory Visibility도 보장한다.
즉, 한 스레드가 수정한 값이 다른 스레드에게 즉시 보이도록 한다.

## Synchronized

### Synchronized 사용법 3가지 케이스

#### 1. 인스턴스 메서드에 붙여 this 모니터를 잠그는 방법
```java
public synchronized void increment() {
    value++; 
}
```
- 메서드 전체가 `this` 모니터로 보호됨.
- 인스턴스마다 락이 걸림.

#### 2. static 메서드에 붙여 Class 객체 모니터를 잠그는 방법
```java
public static synchronized void log(String msg) { ... }
```
- 클래스 전체에 하나뿐인 락(`ClassName.class` 모니터)을 잡으며, 모든 인스턴스가 공유함.

#### 3. synchronized(expression) 블록
```java
public void deposit(int amount) {
    synchronized(lock) {
        balance += amount;
    }
}
```
- 특정 객체(`lock`)를 락으로 사용하여 임계 영역을 좁힐 수 있음.

### Synchronized 성능 개선

#### ReentrantLock
- **Lock Contention(락 경합) 줄이기**: 여러 스레드가 동시에 lock을 얻으려고 할 때 `ReentrantLock`은 `synchronized`보다 효율적으로 동작할 수 있음 (특히 구버전 Java에서).
- 공정성(Fairness) 설정 가능.
- `tryLock()` 등으로 유연한 락 획득 가능.

## Semaphores and Mutex

### Definition of Semaphore
세마포어란, 어떤 Integer S.
initialization을 제외하고, 오직 2개의 atomic 한 operations (`wait`, `signal`)로만 접근 가능.

- **Counting Semaphore**: 제한 없이 값이 올라갈 수 있음 (자원 N개 관리).
- **Binary Semaphore (Mutex)**: 0과 1만 가능. 상호 배제(Mutual Exclusion)를 위해 사용.

### Monitor
모니터 자체는 high-level synchronization construct.
모니터 안에서의 procedure 가 한번에 하나만 active 하는 것을 보장함.
Java의 `synchronized`가 모니터 락을 기반으로 함.

## Explicit Lock

자바 5.0 부터 `ReentrantLock` 지원.
`synchronized` (암묵적 락)와 달리 명시적으로 `lock()`, `unlock()`을 호출해야 함.

### Memory visibility
`ReentrantLock`도 `synchronized`와 동일한 memory-visibility semantics를 제공함.
