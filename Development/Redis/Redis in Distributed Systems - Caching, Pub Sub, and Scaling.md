- [ ] Redis in Distributed Systems: Caching, Pub/Sub, and Scaling ➕ 2025-08-18 

### ||Q|| : Asking about Redis and its usage in high-traffic systems

Redis 가 뭐고 이게 왜 대용량 트래픽을 다루는 시스템에서 항상 쓰이는 것일까

### ||A|| : Answering what Redis is and its role

Redis = Remote Dictionary Server

#### What is Redis? (Redis가 뭔가?)

Redis is an in-memory key-value database that stores data in RAM instead of disk, making it extremely fast. It's primarily used as a cache or quick-response database.

**Key characteristics**

- In-memory storage → ultra-fast read/write operations
    
- Single-threaded but can handle 100,000+ requests per second
    
- Supports various data structures (strings, hashes, lists, sets)
    

#### Is it always used in high-traffic systems? (항상 쓰이는가?)

No, not always. Redis는 선택사항이지 필수가 아님.

**When Redis is commonly used**

- Caching frequently accessed data
    
- Session management for web applications
    
- Real-time analytics and leaderboards
    
- Message queues and pub/sub systems
    

**When you might NOT need Redis**

- Simple applications with low traffic
    
- Systems where data consistency is more critical than speed
    
- Applications with sufficient database performance
    
- Budget-constrained projects (additional infrastructure cost)
    

Bottom line: Redis is popular in high-traffic systems because it reduces database load and improves response times, but it's not mandatory. The decision depends on your specific performance requirements, budget, and system complexity.

많은 대용량 시스템에서 사용되지만, 모든 시스템에 필요한 건 아니야.

### ||Q|| : Asking if Redis stores data in RAM instead of a hard disk like an RDBMS

아 Redis 는 따로 저장공간이 RDBMS 처럼 하드디스크가 있는게 아니라 Ram 에 저장하는거야 ?

### ||A|| : Confirming that Redis is an in-memory database with disk for backup

맞습니다! Redis는 RAM에 데이터를 저장하는 in-memory database입니다.

#### Primary Storage = RAM

Redis는 모든 데이터를 메모리(RAM)에 저장하고, 여기서 직접 read/write 작업을 수행합니다. This is why it's so fast:

- RAM access: ~120 nanoseconds
    
- SSD access: ~50-150 microseconds (1000x slower)
    
- HDD access: ~1-10 milliseconds (10,000x slower)
    

#### Disk = Backup Only

하지만 Redis는 persistence를 위해 disk에도 백업을 저장합니다:

- RDB snapshots: 주기적으로 메모리 데이터를 disk에 저장
    
- AOF logs: 모든 write operation을 disk 파일에 기록
    

Key point: Disk storage는 백업용이지 실제 operation용이 아닙니다. 모든 데이터 접근은 RAM에서만 이루어집니다.

#### Critical Limitation

데이터셋 크기 ≤ RAM 크기

If you have 8GB RAM, you cannot store more than ~8GB in Redis. 메모리 부족시 Redis는 error를 발생시킵니다.

Bottom line: Redis = RAM 기반 + disk backup for recovery. 이것이 RDBMS와 근본적으로 다른 점입니다.

### ||Q|| : Confirming the data restoration process upon restart

그러면 Redis 가 데이터 영속성을 위해 하드디스크에 저장하면, Redis 를 이용하는 시스템이 re-start 될때 필요한 데이터를 램에 다시 가져오겠네 하드디스크에서 ?

### ||A|| : Explaining the restart and data recovery process

정확합니다! Redis restart시 disk에서 RAM으로 데이터를 복원합니다.

#### Restart Process (재시작 과정)

Redis shutdown → 모든 RAM 데이터 손실

Redis restart → disk에서 persistence file을 읽어서 RAM에 로드

Service ready → 모든 데이터가 다시 RAM에 준비완료

#### Two Restoration Methods

- **RDB방식**: .rdb snapshot file을 RAM에 로드
    
    - Fast restore but 마지막 snapshot 이후 데이터는 손실 가능
        
- **AOF방식**: write operation log를 replay해서 복원
    
    - Every write operation replayed → 더 안전하지만 느림
        

#### Critical Point

Loading time depends on data size. 큰 데이터셋의 경우:

- RDB: 상대적으로 빠른 복원
    
- AOF: 모든 operation을 replay하므로 더 오래 걸림
    

Bottom line: Yes, Redis가 restart되면 disk persistence file(RDB or AOF)에서 모든 데이터를 RAM에 다시 로드합니다. 이 과정이 완료되어야 Redis가 정상 서비스 가능합니다.

그래서 persistence 설정이 중요한 이유입니다 - 없으면 restart시 모든 데이터 손실.

### ||Q|| : Asking about the architecture of Redis Pub/Sub for inter-server communication

나는 Redis pub/sub 를 이용해서 서로다른 서버 인스턴스끼리 메시징을 할 수 있게끔했는데, 이게 어떻게 가능한거지 ? 예를들어, 물리적인 서버 1, 서버 2 가 있고 제 3 컴퓨터에 redis 가 있으면 redis 서버가 서버 1, 서버 2와 통신할 수 있게끔 할 수 있는건가 ? 아니면 서버1,서버2에 각각 redis 서버가 돌아가는건가 ?

### ||A|| : Explaining the centralized message broker architecture of Redis Pub/Sub

정확합니다! 당신이 이해한 것이 맞습니다.

#### Typical Architecture (일반적인 구조)

중앙집중식 Redis = Message Broker

Plaintext

```
서버1 ←──→ Redis Instance ←──→ 서버2
서버3 ←──┘                └──→ 서버4
```

하나의 Redis 인스턴스가 제3 컴퓨터에서 실행되고, 모든 서버들이 이 Redis에 네트워크를 통해 연결합니다.

#### How it Works (작동 원리)

- All servers connect to the same Redis instance
    
- Each server subscribes to specific channels (e.g., "notifications", "chat")
    
- When Server1 publishes a message → Redis distributes to all subscribers
    
- Server2, Server3, etc. receive the message simultaneously
    

#### Network Communication

Redis uses TCP connections over network:

- 서버1: `redis.connect('redis-server-ip:6379')`
    
- 서버2: `redis.connect('redis-server-ip:6379')`
    

동일한 Redis 인스턴스에 연결

#### Not Per-Server Redis

일반적으로는 각 서버마다 Redis를 두지 않습니다. 왜냐하면:

- Message silos 발생 - 서버간 통신 불가
    
- Coordination complexity 증가
    

#### Production Considerations

- Single point of failure: Redis 클러스터링 필요
    
- Network latency: Redis 위치가 중요
    
- Load balancing: Multiple Redis instances for scale
    

Bottom line: 하나의 중앙 Redis가 message broker 역할을 하며, 모든 서버들이 네트워크를 통해 이 Redis에 연결해서 pub/sub 통신을 합니다.

### ||Q|| : Asking how to handle publishers receiving their own messages

그런데 redis 에 publishing 하면 publush 한 서버도 그 메시지를 받더라. broadcast라 그런지. 이런 문제는 어떻게 해결하지 보통 ? 난 publish 한 데이터에 서버 고유의 id 를 집어넣어서 해결했어.

### ||A|| : Validating the solution and explaining common patterns

정확한 관찰이고 좋은 해결책입니다! Redis pub/sub는 기본적으로 broadcaster이므로 publisher도 자신의 메시지를 받습니다.

#### Why This Happens (왜 이런 일이 발생하는가)

Redis pub/sub는 fire-and-forget broadcast 방식입니다:

- Publisher가 채널에 메시지를 보내면
    
- 모든 subscribers (publisher 포함)가 메시지를 받음
    
- Redis는 "누가 보냈는지" 구분하지 않음
    

#### Common Solutions (일반적인 해결책)

**1. Your Solution = Server ID (당신의 방법)**

JSON

```
{
  "serverId": "server-1",
  "message": "data here"
}
```

- 각 서버가 메시지 받을 때 자신의 ID와 비교
    
- 같으면 무시, 다르면 처리
    
- Simple and effective
    

**2. Separate Channels per Publisher**

Plaintext

```
server1 → publishes to: "from-server1" 
server2 → subscribes to: "from-server1", "from-server3"
```

- 각 서버마다 전용 발행 채널
    
- 자신의 채널은 구독하지 않음
    

**3. Message Acknowledgment System**

Plaintext

```
publisher → sends with unique messageId
subscribers → acknowledge receipt  
publisher → ignores own acks
```

#### Best Practice Recommendation

당신의 Server ID 방식이 가장 practical합니다:

- Simple implementation
    
- Low overhead (단일 필드 추가)
    
- Reliable filtering
    
- Easy to debug
    

Bottom line: Redis pub/sub의 broadcast 특성상 자연스러운 현상이고, 당신의 server ID 해결책이 industry standard 중 하나입니다. 잘 해결하셨네요!

### ||Q|| : Asking about Redis's importance for databases in a distributed environment

Redis 를 분산환경에서 중요한 이유는 ? client의 화면에 바로바로 변화를 보여주는 것 뿐만 아니라 DB 관련해서도 중요하다고 들었는데.

### ||A|| : Explaining the roles of Redis in a distributed environment for performance and architecture

Redis가 분산환경에서 중요한 이유는 크게 두 가지입니다:

#### 1. Real-time Updates (실시간 업데이트)

In-memory 특성으로 millisecond 응답시간을 제공합니다:

- Traditional DB: disk에서 읽기 → 수십 ms
    
- Redis: RAM에서 읽기 → 1ms 미만
    

Millions of operations per second 처리 가능

Pub/Sub으로 즉각적인 브로드캐스팅:

- 사용자 A가 채팅 → 즉시 모든 연결된 사용자에게 전송
    
- Real-time notifications, live updates 구현
    

#### 2. Database Performance Enhancement (DB 성능 향상)

**Distributed Caching (분산 캐싱)**

가장 핵심적인 역할입니다:

Plaintext

```
Client → Redis Cache (hit) → 즉시 응답 (1ms)
Client → Redis Cache (miss) → DB 조회 → Cache 저장 → 응답
```

**Benefits:**

- DB 부하 80-90% 감소
    
- Frequently accessed data를 RAM에 저장
    
- Database scaling 없이도 성능 향상
    

**Session Management**

분산 서버들이 세션 공유:

- 서버1에서 로그인 → Redis에 세션 저장
    
- 서버2로 요청 와도 → Redis에서 세션 확인 가능
    
- Stateless server architecture 구현
    

#### 3. System Architecture Benefits

**Horizontal Scaling**

Redis Cluster로 데이터를 여러 노드에 분산:

- Sharding: 데이터를 자동으로 여러 서버에 분산
    
- No single point of failure
    
- Linear performance scaling
    

**Data Consistency**

분산 환경에서 shared state management:

- 모든 서버가 동일한 캐시 데이터 접근
    
- Cache invalidation으로 데이터 일관성 유지
    
- Atomic operations 지원
    

#### Real-world Impact

**Without Redis:**

- DB 직접 조회 → 100ms+ 응답시간
    
- 서버별 세션 저장 → 로드밸런싱 문제
    
- 실시간 기능 구현 어려움
    

**With Redis:**

- Sub-millisecond 응답시간
    
- Seamless user experience across servers
    
- Real-time features 쉽게 구현
    

Bottom line: Redis는 분산환경에서 performance bottleneck 해결과 real-time capabilities 제공의 핵심 인프라입니다. DB를 직접 대체하는 게 아니라 성능 가속기 역할을 합니다.

### ||Q|| : Asking if Redis Cluster sharding is the same as DB replication for high availability

## Redis Cluster로 데이터를 여러 노드에 분산: Sharding: 데이터를 자동으로 여러 서버에 분산

이게 바로 그 DB 를 복제해서 가용성을 높이는 방법인거지? DB 한쪽이 failure 해도 여전히 서비스를 제공할 수 있는.

### ||A|| : Differentiating between Sharding and Replication

아니요, Redis Cluster의 Sharding과 가용성을 위한 DB 복제는 다른 개념입니다.

#### Sharding vs Replication (샤딩 vs 복제)

**Sharding (샤딩)**

- 데이터를 분할해서 여러 노드에 다른 데이터를 저장
    
- 예: 사용자 1-1000 → 노드1, 사용자 1001-2000 → 노드2
    
- 목적: 성능 향상, 저장 용량 확장
    
- 특징: 각 노드가 전체 데이터의 일부만 가짐
    

**Replication (복제)**

- 같은 데이터를 여러 노드에 복사해서 저장
    
- 예: 모든 사용자 데이터를 노드1, 노드2, 노드3에 동일하게 저장
    
- 목적: 가용성(High Availability), 장애 대응
    
- 특징: 각 노드가 전체 데이터의 완전한 복사본을 가짐
    

#### Redis Cluster의 실제 구조

Redis Cluster는 샤딩과 복제를 모두 사용합니다:

Plaintext

```
Master1 (데이터A) → Slave1 (데이터A 복제)
Master2 (데이터B) → Slave2 (데이터B 복제)  
Master3 (데이터C) → Slave3 (데이터C 복제)
```

- **Sharding**: 데이터A, B, C로 분할
    
- **Replication**: 각 Master의 데이터를 Slave에 복제
    
- **Result**: 성능 + 가용성 둘 다 확보
    

#### 장애 시 대응 방식

**Pure Sharding (샤딩만)**

- 노드 1개 장애 → 해당 데이터 완전 손실
    
- 가용성 문제 발생
    

**Pure Replication (복제만)**

- 노드 1개 장애 → 다른 노드에서 모든 데이터 제공 가능
    
- 가용성 확보, 하지만 성능 한계
    

**Redis Cluster (샤딩 + 복제)**

- Master 노드 장애 → 해당 Slave가 Master로 승격
    
- 데이터 손실 없이 서비스 계속 제공
    
- 성능과 가용성 모두 확보
    

#### 결론

당신이 언급한 **"DB를 복제해서 가용성을 높이는 방법"**은 Replication에 해당합니다. Redis Cluster의 Sharding은 성능 향상이 주 목적이고, **가용성은 각 샤드의 복제(Replication)**를 통해 달성합니다.

Bottom line: Redis Cluster = Sharding(성능) + Replication(가용성)의 조합입니다.

### ||Q|| : Asking if RAM size is the most critical factor for Redis performance

그럼 Redis 서버는 아무래도 ram 의 크기가 성능에 크게 좌우하겠네. hard disk 나 sdd 보다는.

### ||A|| : Confirming that RAM is the definitive factor for Redis performance

맞습니다! Redis 서버는 RAM 크기가 성능에 결정적으로 좌우됩니다.

#### Redis는 In-Memory 데이터베이스

Redis는 **"in-memory data structure store"**로, 모든 데이터가 서버의 main memory에 저장됩니다. 이는 PostgreSQL, SQL Server 등 대부분의 데이터를 디스크에 저장하는 다른 데이터베이스들과는 근본적으로 다릅니다.

#### RAM vs SSD/HDD 성능 차이

**메모리 접근 속도**

- 캐시 메모리: 1-2 나노초
    
- 메인 메모리: 50-100 나노초
    
- SSD: 20-100 마이크로초 (1,000-5,000배 느림)
    
- HDD: 5-10 밀리초 (50,000-200,000배 느림)
    

#### Redis 성능 이점

Redis가 메모리를 사용하기 때문에 얻는 장점들:

- 초고속 읽기/쓰기: 평균 1밀리초 이하
    
- 초당 수백만 operations 지원 가능
    
- 높은 처리량: 메모리 직접 접근으로 병목현상 최소화
    

#### 메모리 크기가 중요한 이유들

**1. 데이터 전체가 RAM에**

- Redis의 모든 데이터셋이 메모리에 로드되어야 하므로, 가용 RAM 크기 = 저장 가능한 데이터 양
    

**2. Swap 발생 시 성능 급락**

- 메모리 부족으로 swap이 발생하면:
    
- Redis 성능이 HDD/SSD 속도로 급락
    
- 밀리초 → 수십 밀리초로 성능 저하
    

**3. 메모리 사용 패턴**

Plaintext

```
메모리 권장사항:
- 데이터셋 크기의 1.5-2배 RAM 확보
- OS + 기타 프로세스 고려
- Redis persistence (RDB/AOF) 시 추가 메모리 필요
```

#### 스토리지의 역할

HDD/SSD는 Redis에서 보조적 역할만 수행:

- RDB 스냅샷 저장용
    
- AOF 로그 저장용
    
- 서버 재시작 시 데이터 복구용
    

#### 결론

**Redis 성능 우선순위:**

1. RAM 용량 (가장 중요)
    
2. RAM 속도 (DDR4 vs DDR3 등)
    
3. CPU 성능 (연산 처리)
    
4. 네트워크 대역폭
    
5. 스토리지 (persistence용으로만 중요)
    

Bottom line: Redis는 메모리 기반 데이터베이스이므로, RAM 크기와 속도가 성능을 결정하는 가장 중요한 요소입니다. SSD/HDD는 데이터 지속성을 위한 보조 수단일 뿐입니다.

---

### Questions for Review

1. Redis와 전통적인 RDBMS (e.g., PostgreSQL)의 가장 근본적인 차이점은 무엇인가요?
    
2. Redis의 데이터 영속성(persistence)을 보장하는 두 가지 방법(RDB, AOF)을 설명하고, 각각의 장단점을 비교하세요.
    
3. 여러 서버 인스턴스 간의 메시징을 위해 Redis Pub/Sub을 사용할 때, 왜 일반적으로 중앙 집중식 Redis 인스턴스 하나를 사용할까요?
    
4. Redis Pub/Sub에서 메시지를 발행한(publish) 서버가 자기 자신의 메시지를 수신하는 현상을 해결하는 방법 한 가지를 설명하세요.
    
5. 분산 환경에서 Redis가 DB의 부하를 줄여주는 핵심적인 역할은 무엇이며, 어떻게 동작하나요?
    
6. 'Sharding'과 'Replication'의 개념을 설명하고, Redis Cluster가 이 두 가지를 어떻게 조합하여 성능과 가용성을 동시에 확보하는지 설명하세요.
    
7. Redis 서버의 하드웨어 사양을 결정할 때, 왜 CPU나 SSD보다 RAM의 크기를 가장 중요하게 고려해야 하나요?