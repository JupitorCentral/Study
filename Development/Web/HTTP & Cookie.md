---
tags:
  - Web/HTTP
  - Web/Cookie
---

### Chapter 9 - 클라이언트와의 대화 1 : 쿠키

서버는 쿠키를 이용하여 웹 브라우저에 정보 전송 가능.
웹서버로부터 쿠키를 전송받은 웹 브라우저는 이후 웹 서버에 요청을 보낼때 쿠키와 함께 전송함.
이를 사용하면, 웹 서버와 웹 브라우저는 필요한 값을 공유하고 상태를 유지할 수 있음 (stateful)

쿠키는 웹브라우저가 저장하는 데이터.
쿠키는 웹서버, 웹브라우저가 모두 생성할 수 있음.

#### JSP 프로그래밍에서의 쿠키 동작 방식

##### 쿠키 생성

JSP 에서는 웹서버 측에서 생성. 생성한 쿠키를 응답 데이터의 헤더에 저장해서 웹 브라우저에 전송

##### 쿠키 저장

웹브라우저는 받은 쿠키를 쿠키 저장소에 보관.
쿠키 종류에 따라 메모리나 파일에 저장.

##### 쿠키 전송

웹 브라우저는 저장한 쿠키를 요청이 있을때마다 웹 서버에 전송
(해당 쿠키를 생성한 도메인)
웹 서버는 웹 브라우저가 전송한 쿠키를 사용해서 필요한 작업을 수행함.

##### 쿠키의 기본 동작 흐름

**1. 최초 요청 (Initial Request)**

- 웹 브라우저는 처음에 쿠키를 가지고 있지 않습니다
- 서버에 첫 번째 request를 보냅니다[](https://blog.naver.com/with_msip/222960897375)​


**2. 서버 응답 (Server Response with Set-Cookie)**

- 서버는 HTTP response의 `Set-Cookie` header에 쿠키를 포함시켜 보냅니다[](https://www.browserstack.com/guide/cookie-header)​
- 예: `Set-Cookie: sessionId=abc123; Domain=example.com; Path=/`

**3. 브라우저의 쿠키 저장**

- 브라우저는 `Set-Cookie` header를 받으면 해당 쿠키를 저장합니다[](https://hudi.blog/cookie-and-session/)​

**4. 이후 요청 (Subsequent Requests)**

- 동일한 도메인에 대한 이후 요청에서, 브라우저는 자동으로 저장된 쿠키를 `Cookie` header에 포함시켜 전송합니다[](https://httpwg.org/s
pecs/rfc6265.html)​
- 예: `Cookie: sessionId=abc123`

---

웹브라우저에 쿠키가 저장되면, 삭제될때까지 웹 서버에 쿠키를 전송함.
웹 어플리케이션을 사용하는 동안 지속적으로 유지해야 하는 정보는 쿠키를 이용하면 됨.


#### 쿠키의 구성

이름 - 쿠키를 구분
값 - 쿠키의 이름과 관련된 값
유효시간 - 유지 시간
도메인 - 쿠키를 전송할 도메인
경로 - 쿠키를 전송할 요청 경로

브라우저는 유효시간이 지난 쿠키를 자동으로 삭제한다.
별도 유효시간을 설정하지 않으면 웹 브라우저 종료시 쿠키도 함께 삭제함.

또한 지정한 도메인이나 경로로만 쿠키를 전송하도록 제한할 수도 있음.

#### 쿠키 변경

쿠키 생성할떄랑 똑같은 문법.
그래서 쿠키가 존재하는지부터 판단.


#### 쿠키의 도메인

기본적으로 쿠키는 그 쿠키를 생성한 서버에만 전송이 됨. (도메인을 설정 안했을때)
다른 사이트로 연결할때는 전송되지 않고, 생성한 서버에 연결할때만 전송됨.

하지만 같은 도메인 (웹사이트를 기억하기 쉽게 만드는 대표주소, url 은 프로토콜, 경로, 파라미터 등 모든 정보를 다 담은 문자열) 을
사용하는 모든 서버에 쿠키를 보내야 할 때가 있음.

예를 들어, `www.somehosst.com` 서버에서 생성한 쿠키를 `mail.somehost.com` 서버와
`javacan.somehost.com` 서버에 전송해야 할 때가 있음. 이럴떄 setDomain 메서드를 사용함.

setDomain 으로는 현재 서버의 도메인 및 상위 도메인만 전달 가능.
`mail.somehost.com` -> `.somehost.com`은 가능, `www.somehost.com` 은 불가능.

![[Screenshot 2025-10-18 at 7.46.55 PM.png|700]]

쿠키는웹 서버의 도메인과 관련된 쿠키만 저장한다.
그러니까 위에 jsp 파일 결과처럼
브라우저에서 실행되었을때 다른 도메인에 대한 쿠키를 저장한다는 요청을 서버로부터 받아도,
실제로 브라우저는 javacan.tistory.com 에 대한 쿠키는 저장하지 않는다는 얘기.


##### 웹 브라우저가 타 도메인의 쿠키를 받지 않는 이유

보안문제.
예를들어 A 도메인의 서버가 x 라는 쿠키를 가지고 웹페이지 접근을 허용한다고 가정해보자.
이때 임의의 다른 서버 B 에서 해당 쿠키를 수정할 수 있다면, 다른 서버 B 가 서버 A 의 접속 권한을 조작할 수 있게 되는 것이다.

웹 브라우저는 쿠키의 도메인에 따라 쿠키를 서버에 전송한다.

쿠키의 도메인이 .somehost.com 이라 설정되어 있는경우, .somehost.com 에 속해있는 도메인 서버에
모두 쿠키를 전송함. (javacan.somehost.com, `www.somehost.com` 둘 다) .

별도 도메인 설정을 하지 않은 쿠키의 경우 해당 쿠키를 생성한 서버로만 전달됨.

그리고 쿠키는 언제나 다른 도메인에 대한 서버에는 쿠키가 전송이 안된다.


JSESSIONID - jsp/servlet 에서 세션관리를 위해 사용됨.


#### 쿠키의 경로

도메인 -> 쿠키를 공유할 도메인 범위를 지정
경로 -> 쿠키를 공유할 기준 경로를 설정 (?)

setPath() 를 통해 쿠키의 경로 설정
> `http://localhost:8080/chap09/path2/viewCookies.jsp`

위 URL 에서는 `/`, `/chap09`, `/chap09/path2` 등을 쿠키 경로로 사용할 수 있음.

위 메서드를 이용해 쿠키의 경로를 설정하면, 웹브라우저는 지정한 경로 또는 그 하위 경로에 대해서만 쿠키를 전송함.

그래서 예를들어 어떤 쿠키 'myCookie = value1' 에 대한 쿠키의 경로를 `/chap09`로 설정하면,
URL 에 `/chop09` 또는 `chap09/path2` 등 해당 경로 및 그 하위 경로에 request 를 보낼때 쿠키를 전송함.

쿠키에 경로를 설정하지 않으면 실행한 URL 의 경로부분을 사용함.


#### 쿠키의 유효시간

쿠키는 유효시간을 가지며, 유효시간을 설정하지 않으면 웹 브라우저 종료시 쿠키를 함께 삭제함
->

##### Session Cookie vs Persistent Cookie

쿠키의 유효시간에 따라 두 가지 타입으로 나뉩니다:

**1. Session Cookie (세션 쿠키)**

- `Expires`나 `Max-Age` 속성이 **없는** 쿠키입니다[](https://captaincompliance.com/education/session-cookies-vs-persistent-cookies/)​
- 브라우저가 종료되면 자동으로 삭제됩니다[](https://www.cookielawinfo.com/session-cookies-vs-persistent-cookies/)​
- **메모리(cache memory)에만 저장**되고, 하드 디스크에는 저장되지 않습니다[](https://captaincompliance.com/education/session-cookies
-vs-persistent-cookies/)​


```text
`Set-Cookie: sessionId=38afes7a8`
```

이 예시처럼 유효시간(expiration date/time)을 명시하지 않으면, 브라우저를 닫을 때 쿠키가 삭제됩니다.[](https://developer.mozilla.org/
en-US/docs/Web/HTTP/Guides/Cookies)​

**2. Persistent Cookie (영구 쿠키)**

- `Expires` 또는 `Max-Age` 속성이 **있는** 쿠키입니다[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)​
- **하드 디스크에 영구적으로 저장**됩니다[](https://captaincompliance.com/education/session-cookies-vs-persistent-cookies/)​
- 유효기간이 만료되거나 사용자가 수동으로 삭제하기 전까지 유지됩니다[](https://www.cookielawinfo.com/session-cookies-vs-persistent-c
ookies/)​

```text
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT
Set-Cookie: id=a3fWa; Max-Age=2592000
```

##### 쿠키 삭제 주체

**브라우저(Browser)**가 쿠키의 생명주기(lifecycle)를 관리하며, expired된 쿠키를 삭제하는 책임을 가집니다. OS는 이 과정에 관여하지 않
습니다.[](https://en.wikipedia.org/wiki/HTTP_cookie)​

##### 삭제되는 시점 (Deletion Timing)

여기서 중요한 포인트가 있습니다. 쿠키가 삭제되는 **정확한 시점**은 브라우저마다 다르며, 실시간으로 즉시 삭제되지 않을 수 있습니다:[]
(https://connect.mozilla.org/t5/ideas/deleting-of-expired-cookies/idi-p/63379)​

**1. 쿠키가 만료되었을 때**

- `Expires` 날짜가 지났거나 `Max-Age`가 0 이하가 되면 쿠키는 "만료(expired)" 상태가 됩니다[](https://en.wikipedia.org/wiki/HTTP_cook
ie)​
- 하지만 **파일 시스템에서 즉시 삭제되지 않을 수 있습니다**[](https://learn.microsoft.com/en-us/answers/questions/2114084/cookies-th
at-have-expired-are-not-deleted)​


**2. 실제 삭제 시점**

- **요청 시점 (Request Time)**: 브라우저가 다음 HTTP request를 보낼 때, 만료된 쿠키는 전송하지 않습니다[](https://syrenis.com/resour
ces/blog/understanding-cookie-consent-expirations/)​
- **유휴 시간 (Idle Time)**: Chrome은 idle time에 만료된 쿠키를 정리합니다[](https://connect.mozilla.org/t5/ideas/deleting-of-expire
d-cookies/idi-p/63379)​
- **브라우저마다 다름**:

    - Chrome: idle time에 자동으로 expired cookies를 삭제[](https://connect.mozilla.org/t5/ideas/deleting-of-expired-cookies/idi-p/6
3379)​
    - Firefox: 자동 삭제가 제대로 작동하지 않는 경우가 있음[](https://connect.mozilla.org/t5/ideas/deleting-of-expired-cookies/idi-p
/63379)​
    - Edge: 개발자 도구를 열었을 때 삭제되는 경우도 보고됨[](https://learn.microsoft.com/en-us/answers/questions/2114084/cookies-tha
t-have-expired-are-not-deleted)

##### 실제 동작 비교

| 쿠키 타입                 | 저장 위치    | 브라우저 종료 시 | 프로세스 비정상 종료 시 |
| --------------------- | -------- | --------- | ------------- |
| **Session Cookie**    | 메모리(RAM) | 삭제됨       | 삭제 안될 수 있음 ⚠️ |
| **Persistent Cookie** | 하드 디스크   | **유지됨** ✅ | **유지됨** ✅     |



#### 쿠키의 헤더

쿠키를 추가하면 Set-Cookie 헤더를 통해 전달된다.
-> 서버가 쿠키를 생성해서 브라우저에 보낼떄의 이야기.


쿠키는 HTTP 헤더(Header)를 통해 서버와 브라우저 간에 전달됩니다. 여기서 두 가지 다른 헤더가 사용됩니다.[](https://www.browserstack.c
om/guide/cookie-header)​

#### HTTP 헤더와 쿠키의 관계

HTTP 통신에서 쿠키는 메시지의 본문(body)이 아니라 헤더(header) 영역에 포함됩니다. 헤더는 HTTP 메시지의 메타데이터를 담는 부분으로,
요청이나 응답에 대한 추가 정보를 제공합니다.[](https://stackoverflow.com/questions/3467114/how-are-cookies-passed-in-the-http-protoc
ol)​

##### Set-Cookie 헤더 (Response Header)

서버가 브라우저에게 쿠키를 저장하라고 지시할 때 사용하는 헤더입니다.[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/H
eaders/Set-Cookie)

"The HTTP Set-Cookie response header is used to send a cookie from the server to the user agent, so that the user agent can send it
back to the server later."[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)​
​
즉, 서버에서 클라이언트(브라우저)로 가는 방향(Server → Client)에서 사용됩니다

```text
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: sessionId=abc123
Set-Cookie: theme=dark; Max-Age=3600
```

##### Cookie 헤더 (Request Header)

브라우저가 이전에 저장한 쿠키를 서버로 다시 보낼 때 사용하는 헤더입니다.[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Referen
ce/Headers/Cookie)​

"The Cookie header is an HTTP request header used by browsers to send stored cookies back to the server."[](https://www.browserstack
.com/guide/cookie-header)​

즉, 클라이언트(브라우저)에서 서버로 가는 방향(Client → Server)에서 사용됩니다.[](https://www.browserstack.com/guide/cookie-header)​

```text
GET /page HTTP/1.1
Host: example.com
Cookie: sessionId=abc123; theme=dark
```

이 예시에서 브라우저는 HTTP request의 헤더 영역에 Cookie 헤더를 포함시켜 저장된 쿠키들을 서버로 전송합니다.

##### 전체 흐름 예시

RFC 6265에 나온 실제 예시

```text
== Server → User Agent ==
Set-Cookie: SID=31d4d96e407aad42

== User Agent → Server ==
Cookie: SID=31d4d96e407aad42
```

1. 서버가 응답할 때 Set-Cookie 헤더에 쿠키 정보를 담아서 보냅니다
2. 브라우저는 이 헤더를 해석(parse)하여 쿠키를 저장합니다
3. 이후 요청 시, 브라우저는 Cookie 헤더에 저장된 쿠키를 담아서 보냅니다[](https://httpwg.org/specs/rfc6265.html)​

##### 두 헤더의 비교

| 헤더         | 방향              | 역할                   |
| ---------- | --------------- | -------------------- |
| Set-Cookie | Server → Client | 쿠키를 생성하고 브라우저에 저장 지시 |
| Cookie     | Client → Server | 저장된 쿠키를 서버로 전송       |


#### 쿠키를 사용한 로그인 상태 유지

1. 로그인에 성공하면 특정 이름을 갖는 쿠키를 생성
2. 해당 쿠키가 '존재하면' 로그인한 상태라고 상정
3. 로그아웃하면 해당 쿠키를 삭제함


### Chapter 10 클라이언트와의 대화 2 : 세션

세션을 사용하면 클라이언트의 상태를 저장할 수 있는데, 쿠키와 다른 점은
값을 클라이언트의 메모리나 하드디스크에 저장하는 것이 아니라 서버에 저장한다는 점.


JSP는 브라우저마다 세션을 생성한다.
예를 들어 브라우저 A,B 가 있고 jsp 파일 1,2 가 있을때
A 가 jsp 파일 1, 2 를 실행하고, B 도 마찬가지로 jsp 파일 1,2 를 실행한다 하더라도
서버에서는 브라우저 A, 브라우저 B 에 대한 세션만 가지고 있지 페이지에 따라 가지고 있지 않는다.

세션 ID -> 웹브라우저마다 가지고 있는 세션의 고유 ID

웹서버는 쿠키를 통해 웹브라우저와 세션 iD 를 공유함.


세션은 최근 접근 시간을 갖는다.
jsp 에서 session 기본 객체를 사용할때마다 이 시간이 갱신됨.
웹브라우저에서 jsp 페이지를 실행할 때마다 이 session 객체에 접근. (최근 접근 시간 갱신)

세션은 마지막 접근 이후 일정 시간동안 접근하지 않으면 자동으로 세션을 종료하는 기능을 가지고 있음.
(세션 유효 시간)

세션 객체가 지워지지 않으면 memory 부족 현상이 생기므로
반드시 timeout 을 걸어주어야 함.


#### 세션을 사용한 로그인 상태 유지

1. 로그인에 성공하면 session 기본 객체의 특정 속성에 데이터 기록
2. 이후 session 기본 객체에 1번에서 추가한 특정 속성이 존재하는 경우 로그인 한 것으로 간주
3. 로그아웃 할경우 session.invalidate() 메서드를 호출하여 세션을 종료



#### 서블릿 컨텍스트와 세션

톰캣 webapps 에 새로운 폴더 하나 생성 (localhost:8080/chap10, localhost:8080/chap10_2)
동일한 페이지 조회 -> viewCookies.jsp

각 jsp 파일을 2번 실행하면
처음에는 JSessionID 가 없으므로 출력안됨
2번째 실행시 JSESSIONID 가 출력되나,

chap10 아래 viewCookies.jsp 파일을 실행(브라우저에서 접속)한 JSESSIONID 과
chap10 아래 viewCookies.jsp 파일을 실행(브라우저에서 접속)한 JSESSIONID 가 다르다.

-> 이는, 두 경로가 서로 다른 웹 어플리케이션이기 때문.
세션 ID 를 보관할때 사용할 JESSIONID 쿠키의 경로로 웹 어플리케이션의 컨텍스트 경로를 사용함.

웹 어플리케이션의 컨텍스트 경로가 /chap10 이면, 세션을 위한 JSESSIONID 쿠키의 경로도 /chap10 이 됨.
쿠키의 경로는 /chap10 일떄 /chap10 또는 그 하위의 경로에 접근시에만 쿠키를 전송하므로,
/chap10 어플리케이션 에서는 경로가 /chap10 로 설정되어있는 JSESSIONID 만 사용함.

즉 /chap10 와 /chap_02 는 세션을 공유하지 않는다.
