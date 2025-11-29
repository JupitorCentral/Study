---
tags:
  - CS/IO
  - Web/Architecture
---

## Blocking, non-blocking, Synchronize, Asynchronize

메시지 전달 문맥에서는 blocking≈synchronous, non-blocking≈asynchronous로 쓰이기도 하지만
I/O 문맥에선 non-blocking과 asynchronous를 명확히 구분함.

### blocking, non-blocking, asynchronous, synchronous

(공룡책에서)
system call interface 와 연관된 점에 있어서는 blocking i/o, non-blocking i/o 를 사용할 건지가 있다.

application 이 blocking system call 을 호출하면, application 의 실행이 멈춤/지연됨 (suspended)
application 이 OS 의 run queue 에서 wait queue 로 옮겨짐 (상태가 바뀐다는 얘기인듯)
blocking 시스템 콜이 끝난 후에야 다시 applicatioin 이 실행됨
system call 완료되면 실행 재개
실행이 재개되면 i/o 로부터 app 이 값을 전달받고 wait queue -> run queue

i/o device 의 물리적 동작은 일반적으로 asynchronous 하다 - 예측 불가능한 시간을 소모함.


어떤 user-level 프로세스는 non-blocking i/o 가 필요함 - 예를 들면 화면에 데이터를 보여주고 데이터를 처리하는 동안
keyboard 와 mouse input을 받는 user interface.
다른 예로는 동영상의 압축을 풀고 그 결과물을 화면에 표시하면서 동시에 디스크에 있는 파일로부터 프레임을 읽는 비디오 어플리케이션 등
이 있음.

(non-blocking i/o request 는 i/o 의 return 을 기다리지 않고 다른 일을 하러 가는건가? 프로세스가 ?)
nonblocking (system) call 은 어떤 연장된 시간만큼 어플리케이션의 실행을 halt, 즉 중지 하지 않음.
대신에 그 call 은 재빨리, 얼마나 많은 bytes 가 전달됬는지를 알려주는 값을 return 함.
(그러니까 어느정도 시간이 지나고 나면 특정 시간동안 했던 결과물을 return 해 주는 것을 의미하는 듯 함)


---

non-blocking system 의 대안으로는 asynchronous system 이 있다.
asynchronous call 은 i/o 의 완료를 기다리는 것 없이 즉시 return 함.
어플리케이션 코드는 계속 자기 코드를 실행함.
그리고, i/o 가 미래에 완료되고 나면 그때 application 에 알려줌 -
어플리 케이션의 선형의 흐름 밖에서 일어나는 -
어플리케이션의 주소 공간의 어떤 변수를 설정한다거나, signal 이나 software interrupt 나 call-back routine 등의 방법으로 말이다.


non-blocking system 과 asynchronous system 의 차이는
먼저 nonblocking read() 는 어떤 data 가 이용가능하던간에 일단 즉시 값을 읽어오는 것을 의미함.
-> full number of bytes requested, fewer, or none at all
-> 그러니까 요청한 데이터를 다 얻거나, 적게 얻거나 아니면 아예 아무것도 못 얻거나

반면 asynchronous 는 I/O 가 완전히 실행이 되긴 될건데 그게 어플리 케이션이 실행을 막지 않고
다 수행되면 나중에 application 에게 알려 돌려주는 것이 다름.


nonblokcing behavior 의 좋은 예시는 network 소켓에서의 select() system call 임.


동기적 시스템 콜(synchronous system call)은 호출된 작업이 끝날 때까지
호출한 스레드/프로세스가 돌아오지 않는, 즉 완료 시점과 반환 시점이 일치하는 호출.

일반적으로는 blocking 이랑 synchronous call 이 같은 의미로 쓰인다는데...

그럼 도대체 아래의 조합은 뭐지?

서로 다른 프로세스에 대해서 한쪽은 blocking, 한쪽은 asynchronous 하게 뭐 그런건가?

일단 잘 모르겠다.

근데 개념만 제대로 짚고 있으면 어떤 질문이 와도 해결 가능할듯.

- Blocking + Synchronize
- non-blocking + Synchronize
- Blocking + Asynchronize
- non-blocking + Asynchronize

---

Spring 에서는 또 애기가 다르다.

Blocking vs Non‑blocking은 “호출한 스레드가 자원/결과를 기다리며 멈추는가(wait) vs 기다리지 않고 다른 일을 계속하는가
Synchronous vs Asynchronous는 “호출이 완료 시점과 제어 흐름이 결합되어 즉시 결과를 받는가 vs 호출은 즉시 반환하고 완료 통지는 나중에
 콜백/이벤트/퓨처 등으로 받는가”


>AsyncRestTemplate는 “non‑blocking interactions”를 제공하지만 근본적으로 InputStream/OutputStream 기반이라 non‑blocking streaming을
못함 → 비동기 API + 블로킹 I/O



Spring 5 Design patterns 에서는 먼저 Blocking calls 에 대해 얘기한다.

하나의 call 은 동일한 리소스에 대해 다른 쓰레드들이 기다리고 있을떄, 그 자원을 hold 할 수 있다.

> blocking a call means some operations in the application or system that take a longer time to complete, such as file I/O operation
s and database access using blocking drives.

blocking a call -> call 을 blocking 하는 것.


또 Spring 5 Design patterns 에서는 non-blocking calls 에 대해 이렇게 얘기한다.

>A non-blocking API for the resources allows calling the resources without
>waiting for the blocked call such as database access and network calls.

-> nonblocking api 는 db 접근이나 network call 과 같은 blocked call 을 기다리지 않고 resource 를 call 할 수 있게 해준다.
-> 그러니까 resource 가 blocked 이든 아니든 call  자체는 할 수 있지만, 해당 리소스가 blocked 됬다고 해서
쓰레드가 그 resource 를 기다리지 않는 것.

그리고 resource 가 호출시 이용가능하지 않을때, thread 는 blocked 된 resource 를 기다리기 보다 다른 일을 하고,
그 blocked resources 가 이용가능해질때 notified, 즉 알람을 받는다.


드디어 알겠다.
blocking / non-blocking 은 스레드의 상태가 cpu 의 runnable 에서 벗어나는가 아닌가 이다.
즉 sleep 이냐 running 이냐의 차이이다.

synchronous / asynchronous 의 차이는 i/o 등의 요청에 대해서 기다리고 있느냐 아니냐의 차이이다.



### Blocking, Non-blocking, Synchronous, Asynchronous 조합별 결과 획득 방식과 사용 예시

아래 표는 각 조합별로 **결과를 획득하는 방식**(polling, callback 등)과 **적합한 사용 상황**, 그리고 **간단한 예시**를 정리한 것입니
다.

|조합|결과 획득 방식|언제 쓰면 좋은가|예시|
|---|---|---|---|
|**Blocking + Synchronous**|직접 반환값을 기다림 (thread가 대기)|단순/순차적 작업, low traffic, 코드 단순성 중시|`read()`/`write()`
system call, JDBC, `f.read()` in Python|
|**Blocking + Asynchronous**|callback 등록, 하지만 thread는 대기 (비효율적)|드물게 사용, 레거시 async API + 내부 blocking I/O|POSIX
AIO + `aio_suspend()`, AsyncRestTemplate (Spring)|
|**Non-blocking + Synchronous**|polling (반복 상태 확인)|I/O multiplexing, 여러 작업 동시 polling, event loop|`select()`, `poll()`,
Java NIO `Selector`, busy-waiting loop|
|**Non-blocking + Asynchronous**|callback, event, promise 등 (event-driven)|고성능 서버, high concurrency, event-driven, 리액티브|No
de.js `fs.readFile()`, Python `asyncio`, Java WebFlux, Linux AIO + signal|

---

### 각 조합별 간단 설명 및 예시 코드

#### 1. **Blocking + Synchronous**

- **결과 획득:** 함수가 끝날 때까지 thread가 대기, 반환값 직접 사용

```java
/// JDBC로 DB 조회 (blocking + synchronous)
@Repository
public class UserRepository {
    public User findById(Long id) {
        String sql = "SELECT id, name, email FROM users WHERE id = ?";

        // Thread가 DB 응답까지 멈춰서 기다림 (blocking)
        // 결과를 직접 받아서 반환 (synchronous)
        return jdbcTemplate.queryForObject(sql,
            (rs, rowNum) -> new User(
                rs.getLong("id"),
                rs.getString("name"),
                rs.getString("email")
            ),
            id
        );
    }
}

```

##### 🎯 무엇을 하는가?

- DB에 SQL 쿼리를 보내고, **결과가 올 때까지 thread가 멈춰서 기다림**​

- 결과가 도착하면 **그 자리에서 바로 User 객체로 변환해서 반환**​


##### ✅ 어떤 상황에 쓰면 좋은가?

- **게시판, 관리자 페이지** 같은 low traffic 서비스[](https://stackoverflow.com/questions/27991709/what-are-the-advantages-to-blocki
ng-code-over-non-blocking-code)​

- **결제, 주문** 같은 순서가 중요한 transaction[](https://kissflow.com/application-development/asynchronous-vs-synchronous-programmi
ng/)​

- **배치 작업** (데이터 마이그레이션, 야간 정산)[](https://stackoverflow.com/questions/27991709/what-are-the-advantages-to-blocking-
code-over-non-blocking-code)​

- **코드 단순성**이 중요하고, 트래픽이 낮을 때[](https://www.mendix.com/blog/asynchronous-vs-synchronous-programming/)​

#### 2. **Blocking + Asynchronous**

- **결과 획득:** polling (반복적으로 상태 확인)

```java
// WebFlux로 HTTP 요청 (non-blocking + async)
@Service
public class UserService {
    private final WebClient webClient;

    public void fetchUserAsync(Long id) {
        System.out.println("1. 요청 시작");

        // Thread는 즉시 반환 (non-blocking)
        // 결과는 나중에 callback으로 처리 (asynchronous)
        webClient.get()
            .uri("/users/" + id)
            .retrieve()
            .bodyToMono(User.class)
            .subscribe(
                user -> System.out.println("3. 결과 도착: " + user), // 성공 callback
                error -> System.err.println("에러: " + error)       // 실패 callback
            );

        System.out.println("2. Thread는 다른 일을 함");

        // 출력 순서: 1 → 2 → 3
    }
}
```

##### 🎯 무엇을 하는가?

- HTTP 요청을 보내고, **thread는 즉시 반환되어 다른 일을 함**​

- HTTP 응답이 도착하면, **미리 등록한 callback(람다식)이 실행**되어 결과 처리​


##### ✅ 어떤 상황에 쓰면 좋은가?

- **실시간 채팅, 알림, 스트리밍** 같은 event-driven 서비스​

- **마이크로서비스** 간 API 호출 (여러 서비스를 동시에 호출)​

- **높은 트래픽** (동시 접속자 만 명 이상)[](https://techblog.bozho.net/why-non-blocking/)​

- **외부 API 응답이 느린 경우** (200ms 이상 latency)​


#### 3. **Non-blocking + Synchronous**

```java
// NIO Selector로 여러 소켓 polling (non-blocking + sync)
public class NonBlockingSyncServer {
    public void handleConnections() throws IOException {
        Selector selector = Selector.open();
        ServerSocketChannel serverChannel = ServerSocketChannel.open();
        serverChannel.configureBlocking(false); // Non-blocking 설정
        serverChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) { // Polling loop (synchronous)
            // Thread는 멈추지 않고 즉시 반환 (non-blocking)
            int ready = selector.selectNow();

            if (ready == 0) {
                // 아직 준비된 소켓 없음
                Thread.sleep(10); // CPU 낭비 줄이기
                continue; // 다시 확인 (caller가 직접 polling)
            }

            // 준비된 소켓들 처리
            Set<SelectionKey> selectedKeys = selector.selectedKeys();
            for (SelectionKey key : selectedKeys) {
                if (key.isAcceptable()) {
                    // 새 연결 수락
                }
            }
            selectedKeys.clear();
        }
    }
}
```

##### 🎯 무엇을 하는가?

- 여러 소켓을 **반복해서 확인(polling)**하며, 준비된 소켓이 있는지 체크[](https://01.me/en/2014/11/sync-async-blocked/)​

- Thread는 **멈추지 않고(non-blocking)** 계속 돌면서, **caller가 직접 상태 확인(synchronous)**[](https://stackoverflow.com/questions
/26541119/whats-different-between-the-blocked-and-busy-waiting)​


##### ✅ 어떤 상황에 쓰면 좋은가?

- **Nginx, Redis** 같은 I/O multiplexing 서버[](https://01.me/en/2014/11/sync-async-blocked/)​

- **한 thread가 수천 개의 connection을 관리**할 때[](https://01.me/en/2014/11/sync-async-blocked/)​

- **게임 루프** (60 FPS로 상태 반복 체크)[](https://stackoverflow.com/questions/26541119/whats-different-between-the-blocked-and-bus
y-waiting)​

- **임베디드 시스템** (하드웨어 상태 polling)[](https://www.reddit.com/r/embedded/comments/1lmt5y3/blocking_vs_nonblocking_io/)​


#### 4. **Non-blocking + Asynchronous**

```java
// AsyncRestTemplate (Spring, deprecated - 비효율적)
@Service
public class UserService {
    private final AsyncRestTemplate asyncTemplate;

    public void fetchUserAsync(Long id) {
        String url = "http://api.example.com/users/" + id;

        // Callback 등록 (asynchronous)
        ListenableFuture<ResponseEntity<User>> future =
            asyncTemplate.getForEntity(url, User.class);

        future.addCallback(
            user -> System.out.println("결과: " + user.getBody()), // Callback (async)
            ex -> System.err.println("에러: " + ex)
        );

        // 하지만 내부적으로 InputStream.read()는 blocking!
        // → Thread가 HTTP 응답까지 대기 (blocking)
    }
}

```


##### 🎯 무엇을 하는가?

- API 호출 후 **callback을 등록**(async처럼 보임)​

- 하지만 **내부 I/O(InputStream)는 blocking**이라 thread가 실제로는 **대기함**​

- **Async의 장점(thread 반환)도 없고, blocking의 단순함도 없는 비효율적 조합**[](https://dev-yyh.github.io/en/OS/9)​


##### ✅ 어떤 상황에 쓰면 좋은가?

- **거의 쓰면 안 됨!**[](https://dev-yyh.github.io/en/OS/9)​​

- 레거시 코드에서 **실수로** 이런 패턴이 나타날 수 있음

- **대안**: AsyncRestTemplate 대신 **WebClient** 사용​



| 조합                   | 쉽게 설명하면                         | 주요 사용 예시                  |
| -------------------- | ------------------------------- | ------------------------- |
| Blocking + Sync      | 결과 올 때까지기다렸다가바로 받음              | JDBC, 게시판, 결제             |
| Non-blocking + Async | 일단보내놓고다른 일 하다가,나중에 callback으로받음 | WebFlux, 채팅, API 호출       |
| Non-blocking + Sync  | 계속 확인하면서 "끝났어?" 물어봄 (polling)   | Nginx, Redis, 게임 루프       |
| Blocking + Async     | Callback 등록했는데, 내부는기다림(비효율)     | AsyncRestTemplate(쓰지 마세요) |
