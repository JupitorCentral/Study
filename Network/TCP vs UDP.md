---
tags:
  - Network/TCP-UDP
---

## TCP vs UDP



TCP vs UDP 에 대해서 나의 답변은
TCP 는 3way handshake 을 통한 소켓간 연결로 데이터의 순서, 그리고 데이터 신뢰성(데이터가 손실없이 전송되는지) 를 보장하고,
4way handshake 로 연결을 종료함에 따라 오버헤드가 있고,

UDP 는 TCP 와 같은 연결 및 연결 해제 과정 없이 데이터를 전송하며, 네트워크 중간 라우터의 알고리즘 및 네트워크 상황에 따라 어떤 데이
터가 먼저 도착할지 모르기 때문에 데이터 전송 완료 순서를 보장하지 않고, 데이터 신뢰성도 보장하지 않아. 만약 하려면 상위계층해서 해야
해.
대신 연결에 대한 자원이 들지 않기 때문에 상대적으로 데이터 전송속도가 빠르다.

상기 특징때문에 TCP 는 데이터 손실이 반드시 없어야 하는 예를들면 계좌이체 같은 동작시에 필요하고
UDP 는 데이터 손실율을 어느정도 감수할 수 있는 동영상 스트리밍등에 쓰인다.


| 특징      | TCP (Transmission Control Protocol)              | UDP (User Datagram Protocol)            |
| ------- | ------------------------------------------------ | --------------------------------------- |
| 연결 방식   | Connection-oriented (3-way handshake)            | Connectionless (no handshake)           |
| 데이터 신뢰성 | Reliable, ordered delivery (retransmission, ack) | Unreliable, unordered, best-effort only |
| 속도/오버헤드 | 느림 (많은 오버헤드, 헤더 20~60 bytes)                     | 빠름, 최소 오버헤드, 헤더 8 bytes                 |
| 순서 보장   | Yes, sequencing                                  | No, 순서 보장 없음 (상위 계층에서 필요시 구현)           |
| 용도 예시   | 데이터베이스, 계좌 이체, 웹서핑, 이메일                          | 음성/영상 스트리밍, 게임, IoT                     |



TCP 는 자동으로 상대방이 받을 수 있을만큼만 보내지만, UDP 는 받는쪽의 상황을 신경쓰지 않고 데이터를 보내버린다.
따라서 개발할때 uDP 를 사용하면 수신자가 과부하되지 않도록 하는 부분을 application layer 에서 신경을 써야 함


개발자 입장에서는 이 과정을 일일이 구현할 필요가 없음(프로토콜이 자동 처리).
하지만 UDP의 경우, 데이터 손실을 감지하고 재전송하는 로직을 직접 구현해야 하므로 코드 복잡도가 증가


TCP는 내부적으로 네트워크 상태를 추정하고 timeout 값을 동적으로 조정함.
하지만 UDP는 이런 메커니즘이 없으므로, 만약 신뢰성이 필요하다면 애플리케이션 레벨에서 timeout을 설정하고 RTT를 직접 측정해야 함


TCP는 1:1 연결만 가능하지만. 반면 UDP는 "Multicast/broadcast functionality is required" 시나리오를 지원
한 번의 전송으로 여러 클라이언트에게 동시에 데이터를 보낼 수 있음


### 신뢰성 vs 지연시간의 트레이드 오프

신뢰성무조건 최고의 선택지는 아님

DNS 쿼리 같은 초기 도메인 조회나 특정 경우에는 왜 UDP를 더 자주 쓸까?

1. 패킷 수 차이
UDP DNS 쿼리 -> 쿼리 패킷 + 응답패킷 = 2 패킷
TCP DNS 쿼리 -> 3 way handshake 3개 + 쿼리 1개 + 응답 1개 + 연결 종료 2개 -> 최소 7개 패킷

2. 실제 시간 지연 계산
구체적인 수치

UDP DNS 쿼리 (cache miss):
- 연결 설정: 0ms (연결 불필요)
- 쿼리/응답: ~20ms (네트워크 왕복 시간, RTT)
- 총합: **약 20ms**

TCP DNS 쿼리 (cache miss):
- 연결 설정: ~20ms (1 RTT)
- 쿼리/응답: ~20ms (1 RTT)
- 총합: **약 40ms**

-> TCP는 UDP의 2배 지연시간이 발생

이게 왜 문제냐? DNS는 당신이 웹사이트에 접속할 때마다 실행됩니다.
매초 수십억 번 호출됩니다. 이 20ms의 차이가 누적되면 인터넷 전체 성능에 영향을 미칩니다.

4. 신뢰성이 덜 중요한 이유
여기가 핵심입니다. DNS 쿼리의 특성을 생각해봅시다:
        1. 매우 작은 데이터 크기: DNS 쿼리는 매우 작아서 거의 항상 한 패킷 안에 들어옵니다. 따라서 손실될 확률이 낮습니다.omputer-ne
tworks/why-does-dns-use-udp-and-not-tcp/)​
        2. 캐싱이 빈번함: DNS 응답은 여러 레벨에서 캐시됩니다 (브라우저, OS, 라우터, authoritative server). 통계적으로 대부분의 DNS
쿼리는 캐시 히트입니다. 캐시된 결과는 네트워크를 타지 않으므로 손실될 수 없습니다.​
        3. 만약 손실되면?: "UDP has no built-in reliability mechanism such as acknowledgements and retransmission, so it is up to th
e application layer to detect and recover from losses." 즉, DNS 클라이언트는 일정 시간 안에 응답이 없으면 다시 쿼리를 보냅니다(Timeo
ut과 Retry). 이것만으로도 충분한 신뢰성을 제공합니다.​

5. DNS는 사실 둘 다 씁니다 (흥미로운 사실)

"DNS is one of the most critical pieces of internet infrastructure, but it uses both UDP and TCP depending on the situation."[](http
s://arpitbhayani.me/blogs/dns-udp-tcp/)​

응답 크기가 512바이트를 초과하면? TCP를 사용합니다. Zone transfer처럼 대용량 데이터가 필요하면? TCP를 사용합니다.

즉, DNS는 전략적으로 프로토콜을 선택합니다:

- 빠른 쿼리: UDP (대부분의 경우)
- 큰 응답 또는 신뢰성이 중요한 작업: TCP

---

DNS 가 UDP 를 주로 쓰고 또 특정한 경우 TCp 를 쓰는 이유는,
일단 DNS 가 전송할 데이터가 매우 적은데 비해 TCP 통신을 위한 패킷이 옮겨야 할 패킷보다 훨씬 많은 경우에는 오버헤드가 비효율적으로 커
져서 UDP 를 쓰는거고, 또 이걸 잃어버린다고 해도 재전송 요청하는게 간단하기 떄문에 그런거고.

그런데도 DNS 가 TCP 를 사용하는 경우에는 전송해야할 데이터가 크며, 이때 데이터는 신뢰성이 필요하고 UDP 의 경우 데이터를 분할한 데이
터그램중에 하나라도 비트가 잘못 전송되면 데이터그램 자체가 다 손실되기 때문에 전체 데이터를 재전송해야 하고, (TCP 는 일부만 재전송
할 수 있다) 그리고 UDP 는 잘못 전송되면 서로 다른 데이터그램이 섞일수있다.

이때 UDP 의 Datagram 중 조각 중 하나가 손실되어서 Datagram 자체가 버려진다는 의미는
```java
애플리케이션 계층 (5~7):     데이터
                          ↓
트랜스포트 계층 (계층 4):     UDP Datagram (이건 손실 안 됨 = 최소 전송 단위)
                          ↓
네트워크 계층 (계층 3):      IP Packet ← ★ 여기서 Fragmentation 발생!
                          ↓
데이터 링크 계층 (계층 2):   Frame
```
즉 프레임이나 ip packet loss 가 발생했을 때이다

그리고 uDP 는 하나의 데이터그램이 보낼 수 있는 최대 크기가 작다.
그리고 UDP 는 신뢰성 보장이 없기 떄문에 크기가 커질수록 상위 레벨에서 재전송을 요청해야 하는 데이터의크기도 커지겟지.


## TCP 는 어떻게 신뢰성을 보장하는가


### TCP 연결시 동작

client -> server 에 syn 비트가 1인 tcp 세그먼트가 전송됨
초기 시퀀스 번호를 임의로 설정, 이는 clientisn, 이를 시퀀스 필드에 넣는다
예를 들어 clientisn = 42라면, 클라이언트가 전송하는 첫 번째 세그먼트의 시퀀스 번호 필드에 42가 들어감


SYN 세그먼트가 서버 호스트에 도착하면, 서버측 TCP는 연결을 위한 버퍼와 변수를 할당.
그리고 클라이언트에게
다시 새로운 세그먼트를 보내는데 (TCP 헤더만 있는) 이 세그먼트의 SYN 비트도 1로 설정
서버도 자신의 초기 시퀀스 번호 serverisn을 임의로 선택하여 시퀀스 번호 필드에 넣음
이 세그먼트는 SYNACK 세그먼트라 부름

이 응답이 의미하는 바는: "당신의 SYN 패킷을 받았고, 초기 시퀀스 번호는 clientisn입니다. 나는 이 연결을 수립하는데 동의하며, 내 초기
시퀀스 번호는 serverisn입니다.


SYNACK 세그먼트를 받은 클라이언트는 마지막 단계를 수행합니다:

- 클라이언트도 연결을 위한 버퍼와 변수를 할당합니다
- 서버에게 세 번째 세그먼트를 전송하는데, 이는 서버의 SYNACK을 승인합니다
- **ACK 필드에 serverisn+1을 넣습니다**
- **SYN 비트는 0으로 설정합니다** (이미 연결이 이루어졌기 때문)
- 이 세 번째 세그먼트는 **클라이언트 데이터를 포함할 수 있습니다**


#### 왜 3-way인가?

여기서 중요한 질문이 있습니다: **왜 2-way가 아니라 3-way일까요?**

2개 단계만으로도 충분하지 않은 이유는, 서버가 클라이언트의 초기 시퀀스 번호를 받았다는 것을 클라이언트도 확인해야 하기 때문입니다.
만약 2단계만 있다면, 클라이언트는 서버가 자신의 시퀀스 번호를 제대로 받았는지 알 수 없습니다. 3번째 단계의 ACK가 이 확인 역할을 합니
다.

TCP는 데이터를 바이트 단위의 순서가 있는 스트림(stream of bytes)으로 봅니다. 그리고 시퀀스 번호는 세그먼트가 아니라 바이트 스트림 위
치를 추적합니다. 어린이도 이해하는 설명으로는: TCP가 보내는 모든 데이터를 일렬로 늘어놓은 다음, 각 바이트에 번호를 매기는 것입니다.
-> 세그먼트 = TCP header + 보내고자 하는 bytes


### TCP 연결 해제시 동작

- 클라이언트의 FIN 전송 (Step 1)

클라이언트가 더 이상 보낼 데이터가 없다는 의미를 전달합니다. FIN 세그먼트는:
FIN 비트가 1로 설정됩니다
데이터를 포함하지 않을 수 있습니다
클라이언트 TCP는 FINWAIT1 상태로 진입합니다

- 두 번째 단계: 서버의 ACK 응답 (Step 2)
클라이언트의 FIN을 ACK로 승인합니다
(클라이언트가 fin 세그먼트를 보내면 서버가 ack 세그먼트를 리턴함)
서버 TCP는 CLOSEWAIT 상태로 진입합니다

 - 서버의 FIN 전송 (Step 3)

서버도 더 이상 보낼 데이터가 없으면:

"The server then sends its own shutdown segment, which has the FIN bit set to 1."​
서버가:
자신의 FIN 비트를 1로 설정한 세그먼트를 보냅니다
서버 TCP는 LASTACK 상태로 진입합니다

- 네 번째 단계: 클라이언트의 ACK 응답 (Step 4)
마지막으로 클라이언트가:

"Finally, the client acknowledges the servers shutdown segment. At this point, all the resources in the two hosts are now deallocate
d..."​

클라이언트가:
서버의 FIN을 ACK로 승인합니다
클라이언트 TCP는 TIMEWAIT 상태로 진입합니다

#### TIMEWAIT 상태는 왜 필요한가?

여기서 중요한 질문이 있습니다. 클라이언트가 ACK를 보내면 왜 바로 연결을 끝내지 않고 TIMEWAIT 상태에서 기다릴까요?

"The TIMEWAIT state lets the TCP client resend the final acknowledgment in case the ACK is lost. The time spent in the TIMEWAIT stat
e is implementation-dependent, but typical values are 30 seconds, 1 minute, and 2 minutes."​

쉽게 말하면:

만약 클라이언트가 보낸 마지막 ACK가 네트워크에서 손실되면, 서버는 타이머 만료 후 FIN을 다시 보냅니다. 클라이언트는 TIMEWAIT 상태에
있다가 이 재전송된 FIN을 받고 다시 ACK를 보낼 수 있습니다. 만약 TIMEWAIT 없이 바로 연결을 닫았다면, 이 재전송된 FIN을 받을 준비가 안
 되어 있을 것입니다.




#### 왜 4 way 인가?



#### 할당된 메모리와 변수 해제 타이밍

클라이언트는 **TIMEWAIT 타임아웃 완료 후**에 메모리와 변수를 해제

서버의 메모리/변수 해제는:
마지막 FIN을 보낸 후 아니라 클라이언트의 마지막 ACK를 받은 후입니다

클라이언트: FIN → 시간 대기 → 메모리 해제
서버: FIN 전송 → ACK 대기 → 메모리 해제



### TCP 연결 후 데이터 송수신 동작

**Fast Retransmit** 메커니즘입니다:

"A duplicate ACK is an ACK that reacknowledges a segment for which the sender has already received an earlier acknowledgment... If t
he TCP sender receives three duplicate ACKs for the same data, it takes this as an indication that the segment following the segment
 that has been ACKed three times has been lost."​

예시로 정리하면:

- 송신자가 세그먼트 0, 1, 2, 3을 순서대로 전송

- 세그먼트 1이 손실됨

- 수신자는 세그먼트 0을 받음 → ACK 1000 전송 (다음으로 필요한 바이트가 1000부터라는 뜻)

- 세그먼트 2 도착 (순서가 안 맞음) → 다시 ACK 1000 전송

- 세그먼트 3 도착 (순서가 안 맞음) → 다시 ACK 1000 전송

- 송신자가 같은 ACK를 3개 받음 → 세그먼트 1이 손실되었구나!


그러면 송신자는 세그먼트 1(시퀀스 번호 1000~1999)을 재전송합니다.

1. "그럼 Cumulative Acknowledgment(누적 승인)는 정확히 뭔데?" → 이전 모든 바이트가 정상 수신했다는 것만 확인한다는 뜻입니다.

2. "TCP가 순서가 안 맞는 세그먼트(out-of-order segment)를 받으면 버리는 건가, 버퍼에 저장하는 건가?" → 책에서는 프로그래머 선택이라
고 했는데, 실제로는 대부분 버퍼에 저장합니다.

3. "그럼 32-bit 시퀀스 번호는 왜 필요한가? 16-bit는 안 되나?" → 순환(wrap around)할 때 예전 패킷과 헷갈릴 수 있습니다.
