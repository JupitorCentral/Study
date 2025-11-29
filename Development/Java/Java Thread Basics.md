--- 
tags:
  - Java/Thread
---

### 쓰레드 처리
notify
nofityAll
wait
wait(long timeout)
wait(long timeout, int nanos)


#### Notify
-> 이 객체의 모니터에 대기하고 있는 단일 쓰레드를 꺠운다.

모니터 ? 쓰레드 ? 단일 쓰레드 ?


## Thread

^b6c2eb

프로그램 -> 코드의 집합. 이를 메모리에 옮기면 실행이 가능해지며, 이를 '프로세스' 라고 한다.
하나의 '프로세스' 내부에서 자원을 공유하는 더 작은 실행단위를 만들 수 있는데 이를 '쓰레드' 라고 함.

JVM -> 32MB ~ 64MB
쓰레드 하나 추가 -> 1MB 
그래서 쓰레드를 경량 프로세스 (lightweight) 라고도 함

### Thread 를 구현하는 2가지 방법

Runnable interface, Thread class 

Thread class 는 Runnable interface 를 구현한 것.
Java 에서는 상속을 1가지 클래스밖에 받지 못하는데, 
만약에 상속받으려고 하는 부모 클래스가 Thread 를 상속받지 않고 있다면 ?
이때 Runnable interface 를 사용하면 되는 것. 
Thread class 를 상속받지 않고도 Thread 를 사용할 수 있게끔 하기 위한 장치.

- run
  -> 쓰레드가 시작될때 수행되는 동작을 선언하는 코드
  
  *Thread 는 run 코드가 실행을 끝마치기 전까진 끝나지 않음* 
  
- start 
  -> 쓰레드를 실제로 실행시키는 코드

### Thread 생성자

(Runnable) target, (String) name, (ThreadGroup) group, (long) stackSize

target -> Runnable 을 구현한 클래스의 객체
name -> 생성하고자 하는 Thread 의 이름
ThreadGroup -> 쓰레드를 묶을 수 있는데, 묶고 나면 이 ThreadGroup 을 통해 이 그룹에 묶인 쓰레드들에 대한 여러 정보를 확인할 수 있음
stackSize -> 스택의 크기. 경우에따라 무시될 수 있음

> [!note] 여기서 말하는 Stack 은 Collection 의 Stack 과 전혀 상관 없음
> 
>  Java process 시작 -> Runtime data area 구성. 그 중 하나가 스택. 쓰레드 생성시 별도의 스택이 할당됨.
> Java Language Speicification, http://docs.oracle.com/javase/specs 참조.


### sleep

Thread 의 static method -> 보통 해당 메소드를 위함이 아님.
예외 -> sleep

Try-Catch

> [!warning] main 메소드가 끝나더라도, main 또는 다른 메소드에서 시작한 쓰레드가 끝나지 않으면 자바 프로세스는 종료되지 않음
> 
>  daemon 쓰레드는 예외


### Thread 의 주요 메서드들

속성 확인, 지정 

id, name, priority, daemon, stacktrace, state, threadGroup

상태 통제

state, join, interrupt
### What is Daemon Thread ?

보통 Thread 는 실행이 끝날때까지 JVM 종료되지 않음
Daemon Thread 는 실행이 안끝나도 JVM 이 종료될 수 있음. (단, start 로 실행되기전에 daemon 으로 설정되어야 함)



### Synchronized

for Thread safety, we need to use 'synchronized'

여러개의 쓰레드가, 단일 객체의 인스턴스 변수를 바꾸는 메소드에 접근할때 문제가 발생할 수 있음.

#### 사용방법
메소드 자체, 특정 코드만

#### 선언되면 ?
-> 한번에 하나의 쓰레드만 해당 메소드 / 코드에 접근/실행 할 수 있다.

#### 성능이슈
메소드 전체를 synchronized 처리하면, 성능상 이슈가 발생할 수 있음
-> Thread safety 하지 않아도 되는 코드들 까지도 쓰레드가 기다려야 함.


#### lock 객체 

```java
Object lock = new Object();   // 보통 이렇게 잠금처리를 위한 object 를 선언 -> '문지기'

public void plus (int value) {
	synchronized(lock) {
		amount += value;
	}
}

public void minus (int value) {
	synchronized(lock) {
		amount -= value;
	}
}
```

lock 객체가 한 개의 쓰레드만 이용하게 허락함

그런데 lock 역할을 하는 객체를 여러개 선언해야 할 때도 있음

method1, method2 가 모두 synchronized 기능이 필요한데, 동일한 lock 객체를 사용하게 되면, 
method1 이 락이 걸릴때 method2도 락이 걸려버림.


#### 실수

```java

// 합리적인 사용
CommonCalculate calc = new CommonCalculate();
ModifyAmountThread thread1 = new ModifyAmountThread(calc, true)
ModifyAmountThread thread2 = new ModifyAmountThread(calc, true)


// synchronized 가 필요없음 -> 단일 객체의 메소드에 접근하는 것이 아니기 때문.
CommonCalculate calc1 = new CommonCalculate();
ModifyAmountThread thread1 = new ModifyAmountThread(calc1, true)

CommonCalculate calc2 = new CommonCalculate();
ModifyAmountThread thread2 = new ModifyAmountThread(calc2, true)

```


### Thread 를 통제하는 메소드

getState, join (milllis, nanos), interrupt


#### Join

```java
thread1.start();  
thread2.start();  
  
try {  
    thread1.join();  
    thread2.join();  
    System.out.println("Final Value is " + calc.getAmount());  
}
```

> [!info] 여기서, ==기본 main thread== 가 thread1, thread2 를 start 시키고, 그 쓰레드 자체가 thread1, thread2  를 Timed Waiting 상태로 변경시키는것.

#### State enum class
new, 

runnable -> Ready , Running

Ready -> (thread is selected by scheduler) -> Running
scheduler 는 어떤 CPU 가 어떤 코드를 실행할지 결정한다. (CPU 에 할당함)

Runnig -> (stopped by scheduler) -> Ready
또한, 알고리즘에 따라 scheduler 는 해당 코드의 실행을 그만두고, (CPU 에 다른 코드를 할당함) Ready 상태로 변환

timed_waiting
waiting
blocked

terminated 

![[Pasted image 20250906205131.png|700]]


#### interrupt

-> InterruptedException 을 발생시키면서 중단시킴.
-> sleep, join (-> Timed Waiting)

-> 쓰레드가 시작하기전 또는 terminated 된 상태라면 ?


join (500) -> 0.5 초 기다림

만약 sleep(2000) 을 run 하고, thread 를 start 시킨다음, join(500) 을 하면, 
1.5초 후에 join 다음의 코드가 실행된다.
이때 해당 thread.interrupt 를 실행시키면, InterruptedException 오류 발생 

wait 과 join 의 차이점은, Main Thread 를 기다리게 하냐 마냐의 차이같다.

#### Object 에서의 thread 관련 메소드

^dd13d0

wait -> 대기
notify -> 대기 해제
notifyAll

##### wait & notify 

wait ()
wait (int timeout)

timeout 이 없으면 무한정 대기 (Waiting, not TimedWaiting)
-> 해당 쓰레드의 monitor (lock 을 담당하는 instance variable) 이 notify, notifyAll 을 해주어야만 Runnable 로 상태가 바뀐다

notify 는 Timed Waiting 상태도 깨운다


```java

Thread.sleep(100)  // -> main thread 겠지 ? 정체가 필요하다

synchronized(monitor) {
	monitor.notify()
}
```

##### 여기서, synchronized 가 필요한 이유는 무엇일까 ?

^a7bfed


## ThreadLocal

ThreadLocal 이란, 쓰레드가 공통된 객체를 통해 다른 쓰레드와는 공유되지 않는 독자적인 공간에 값을 저장할 수 있게 만들어 주는 도구라고 볼 수 있다.

Spring 을 예시로 들자면, Thread 는 Request 가 들어오면 Tomcat 의 Thread Pool 에서 해당 request 를 위한 Thread 가 생성된다. (이 Thread 를 WorkThread 라고 부름)

이 workThread 는 하나의 스프링어플리케이션에서 여러개 있을 수 있고, 이 쓰레드들은 static 으로 선언된 ThreadLocal 변수에 대해

Thread 객체의 set, get 을 호출하여 쓰레드마다 임의의 객체를 저장할 수 있고,
이 객체들은 각자의 Thread 메모리 공간에 저장되며, 다른 쓰레드와 그 공간을 공유하지 않는다.

ThreadLocal 을 사용할 경우 각 쓰레드에는 ThreadLocalMap 이라는 곳에 관련된 정보가 저장된다.

### spring  예시

```java
@Component
public class MyWorker {
    public static ThreadLocal<String> threadLocal1 = new ThreadLocal<>();
    public static ThreadLocal<Integer> threadLocal2 = new ThreadLocal<>();

    public void process(String str, Integer num) {
        threadLocal1.set(str);
        threadLocal2.set(num);
        // 실제 작업
    }

    public String fetchThreadLocal1() {
        return threadLocal1.get();
    }
    public Integer fetchThreadLocal2() {
        return threadLocal2.get();
    }
}
```

MyWorker 는 @Component 이기때문에 Spring의 singleton bean 으로 spring 어플리케이션동안 객체가 존재한다.

```java
@Service
public class MainService {
    @Async
    public void workInThread(String name, Integer count) {
        MyWorker worker = new MyWorker();
        worker.process(name, count);
        // ThreadLocal 값 사용
    }
}

...
mainService.workInThread("foo", 100); // worker thread-1
mainService.workInThread("bar", 200); // worker thread-2
...

```

결과값.

| ThreadName      | Key (ThreadLocal instance) | Value |
| --------------- | -------------------------- | ----- |
| worker-thread-1 | threadLocal1               | "foo" |
|                 | threadLocal2               | 123   |
| worker-thread-2 | threadLocal1               | "bar" |
|                 | threadLocal2               | 456   |
각 쓰레드에 대해서, ThreadLocalMap 에는 ThreadLocal 변수 명이 Key, Value 는 그 변수에 저장된 객체가 된다.


### 의문점 - Thread instance variable

Runnable interface 를 구현하는 Thread 의 객체에는 instance variable 을 설정해
각 객체마다 다른 쓰레드 객체와는 공유하지 않은 고유의 변수, 즉 메모리 - 객체를 저장할 수 있는데
왜 굳이 ThreadLocal 을 쓰는가?

왜냐하면, Spring Application 과 같은 다중 쓰레드 환경에서는
적어도 Spring 에서는, Framework 에서 쓰레드를 설정하지 쓰레드 자체를 개발자가 개조하지 않기 떄문이다.

Tomcat Thread Pool 에서 Request 에 생성되는 쓰레드에 대해서 개발자가 해당 쓰레드의 instance variable 을 추가할 수가 없다.

Spring Aplpication 에서는 ThreadPool 이 Thread 를 직접 생성 및 관리한다.
Spring이 일아서 한다. (개발자가 `new Thread()` 로 선언하여 생성하지 않는다는 얘기.)

만약 Spring Application code 에서 new Thread() 를 사용하면,
일반적인 하나의 자바 소스코드와는 다르게

WorkThread 에서 해당 Thread 를 호출, 생성하는 것이므로
Request 2개에 대해 각 workThread 가 임의의 쓰레드를 2개씩 생성하면
총 6개의 쓰레드가 생성이 되나
이 새로 생성된 쓰레드는 사실상 의미가 없다.
그리고 사용하더라도 관리가 힘들다 (그럴 것으로 예상된다. 그리고 어따가 써먹게?)

또한, Spring 빈/컨트롤러/서비스는 싱글톤 개념을 기반으로 사용되며 Thread 생명주기와 별도로 관리된다.
만약 Thread 객체에 직접 인스턴스 변수를 넣으면, Spring 구조(빈, DI, IoC)와 전혀 상관없으므로 프레임워크 설계상 추천되지  는다.
(즉 앞뒤가 안맞는다.)


즉, Spring Application 에서 어떤 쓰레드가 쓰레드만의 변수, 메모리 공간을 가지기 위해서는
ThreadLocal 이 가장 크게 유용한 수단이다.

다른 수단들도 있긴한데 ThreadLocal 에 비해서 단점이 명확하고 크다.

#### Spring 에서의 다른 수단

- **Request/Session/Prototype Scope Bean**
    Spring에서 @Scope("request") 또는 @Scope("prototype")을 사용하면 요청마다 혹은 인스턴스마다 새로운 bean을 생성할 수있음.
    단, child thread에는 scope가 전파되지 않아 한계가 명확.

- **동기화 (synchronized, Lock)**
    멤버 변수 공유를 막으려면 동기화 기법, java.util.concurrent 컬렉션 등으로 동시에 한 쓰레드만 접근하도록 제어할 수 있음.
    그러나, thread별로 분리되는 게 아니고 state 자체가 공유되어 thread 안전만 보장하는 수준.

- **불변 객체(Immutable Object) 활용**
    상태를 아예 변경 불가능하게 두고, 필요한 새 값을 복사해 전달함.
    단, 각 thread별로 상태값이 달라야 하는 상황에서는 실질적으로 곤란.

- **JDK 20+ ScopedValue**
    ThreadLocal을 대체하는 ScoppedValue라는 새로운 기능(JDK20 이상)이 존재하며, 지정된 코드 block 내에서 thread별로 값을 세팅할 수 있습니다.
    아직 Spring 등에서 상용패턴으로는 널리 쓰이지 않습니다.

- **Actor/Message Passing 방식 적용**
    Akka 등 Actor 모델을 써서, 아예 상태 공유하지 않고 메시지 패싱으로 격리.
    Spring 기존 구조와는 동떨어져 있고, 전면 아키텍처 대체에 가까움.

-> 각 방법에 대해서는 아직 잘 모른다.



### When to get GC ?

먼저, ThreadLocalMap 의 key 의 ThreadLocal 에 대한 참조는 WeekRefernece 이다.
따라서 ThreadLocal 이 어플리케이션에서 Strong Reference 가 모두 사라지면, GC 의 대상이 된다.

#### Weak Reference 를 사용하는 이유

개발자가 외부 코드에서 ThreadLocal 인스턴스에 대한 참조를 제거하는 즉시,
GC가 다음 주기에서 해당 ThreadLocal 키를 회수할 수 있으며,
이는 ThreadLocal 변수가 더 이상 불필요할때 자동으로 정리되어

장기간 실행되는 스레드 환경에서 메모리 누수 방지에 필수적임.

이 개념은 `WeakHashMap`이 키에 대해 약한 참조를 사용하여 키가 GC의 대상이 되면 맵의 항목이 제거되도록 하는 것과 동일한 맥락.
-> 이건 또 처음들어본다.

#### ThreadLocal 은 언제, 누구한테서 Strong Reference 되는가?

```