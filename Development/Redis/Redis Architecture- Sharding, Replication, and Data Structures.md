- [ ] Redis Architecture: Sharding, Replication, and Data Structures ➕ 2025-08-18 

### ||Q|| : Asking about the master-slave relationship in Redis with sharding and replication

Redis 를 이용한 sharding, replication 을 혼용하는 방법에서, master db, slave db 간의 관계가 어떻게 이루어지는지 자세히 알고 싶습니다.

### ||A|| : Explaining Redis Cluster architecture for sharding and replication

Redis sharding과 replication을 혼용할 때의 master-slave 관계에 대해 자세히 설명하겠습니다.

#### Redis Cluster Architecture (Redis 클러스터 아키텍처)

Redis에서 sharding과 replication을 함께 사용하려면 **최소 6개의 노드가 필요**합니다. 이는 3개의 master shard와 각 shard마다 최소 1개의 replica로 구성됩니다.

#### Master-Slave Relationship Structure (마스터-슬레이브 관계 구조)

  * **Shard-based Organization (샤드 기반 구성)**

      * 각 shard는 1개의 primary(master) 노드와 여러 개의 replica(slave) 노드로 구성됩니다.
      * Primary 노드는 read/write operations을 처리하고, replica 노드들은 read-only operations만 처리합니다.
      * 전체 데이터는 16,384개의 hash slots로 분할되어 각 shard에 분산 저장됩니다.

  * **Data Distribution (데이터 분산)**

      * `Key → CRC16(key) mod 16384 → Hash Slot → Specific Shard`
      * 각 key는 `CRC16(key) mod 16384` 공식으로 특정 hash slot에 매핑됩니다.
      * Hash slot은 shard에 할당되어, 해당 shard의 master가 해당 데이터를 담당합니다.

#### Replication Mechanism (복제 메커니즘)

  * **Replication Process (복제 과정)**

      * 각 Redis master는 고유한 replication ID와 offset을 가집니다.
      * Replica들은 `PSYNC` 명령어를 사용하여 master와 동기화합니다.
      * Asynchronous replication이 기본값으로, master는 replica의 처리 완료를 기다리지 않습니다.

  * **Synchronization Types (동기화 유형)**

      * **Full Synchronization (전체 동기화):**
          * Master가 background에서 RDB 파일을 생성합니다.
          * 동시에 새로운 write commands를 buffer에 저장합니다.
          * RDB 파일을 replica에 전송하고, replica가 이를 메모리에 로드합니다.
          * Buffer된 commands를 replica에 스트림으로 전송합니다.
      * **Partial Synchronization (부분 동기화):**
          * Replica가 연결 중단 후 재연결할 때 사용됩니다.
          * Master의 backlog buffer에 충분한 데이터가 있으면 증분 데이터만 전송합니다.

#### Fault Tolerance (장애 허용성)

  * **Automatic Failover (자동 장애 조치)**

      * Master 노드가 실패하면, 해당 shard의 replica 중 하나가 자동으로 master로 승격됩니다.
      * Redis Enterprise는 replica shard가 항상 다른 물리적 노드에 배치되도록 보장합니다.

  * **Multi-level Replication (다단계 복제)**

      * Redis는 multilevel master-slave replication을 지원합니다.
      * Slave가 다른 slave들의 master 역할을 할 수 있어 계층적 복제 구조가 가능합니다.

#### Practical Example (실제 예시)

6개 노드로 구성된 Redis Cluster:

  * Shard 1: Master A → Slave A1

  * Shard 2: Master B → Slave B1

  * Shard 3: Master C → Slave C1

  * **Write operations:** 해당 key의 hash slot을 담당하는 master에게만 전송

  * **Read operations:** Master 또는 해당 shard의 replica에서 처리 가능

  * **Data consistency:** Master에서 replica로 asynchronous replication으로 일관성 유지

#### Performance Benefits (성능상 이점)

  * **Sharding:** Write operations의 horizontal scaling 제공
  * **Replication:** Read operations의 horizontal scaling 제공
  * **Combined:** 읽기와 쓰기 모두에서 확장성과 고가용성을 동시에 달성

이러한 구조를 통해 Redis는 대규모 데이터와 높은 처리량을 요구하는 환경에서도 안정적이고 확장 가능한 성능을 제공할 수 있습니다.

-----

### ||A|| : Summarizing and confirming understanding of Redis sharding and replication

그러니까 redis 가 특정 데이터에 대해서 hash를 이용해 나누어서 3개의 db에 나눠서 저장하고, 그 db 는 background 프로세스로 각각의 master db 를 slave db에 replication 한다는 얘기로 들리는데 맞아?

### ||A|| : Correcting and refining the understanding of Redis Cluster

네, 기본적인 개념은 맞지만 몇 가지 정확하지 않은 부분이 있어서 정정하겠습니다.

#### 정확한 Redis Cluster 구조 (Correct Redis Cluster Structure)

  * **Sharding 방식 (데이터 분산)**

      * Redis는 전체 키 공간을 **16,384개의 hash slot**으로 나눕니다.
      * 각 키는 `CRC16(key) mod 16384` 공식으로 특정 hash slot에 매핑됩니다.
      * 3개가 아니라 임의의 개수의 master node에 이 hash slot들을 분배할 수 있습니다.
      * 예를 들어, 3개 노드의 경우:
          * Node A: hash slots 0-5500
          * Node B: hash slots 5501-11000
          * Node C: hash slots 11001-16383

#### Master-Replica 관계 (마스터-복제 관계)

  * 당신이 말한 것처럼, 각 master 노드는 background process로 replica에 데이터를 복제합니다. 하지만:
      * 각 shard(샤드)마다 master-replica 관계가 존재합니다.
      * "Database"가 아니라 \*\*"shard" 또는 "node"\*\*라고 부르는게 정확합니다.
      * 최종 구조는 A, B, C (masters) + A1, B1, C1 (replicas)가 됩니다.

#### 실제 동작 과정 (Actual Process)

  * **Write Operation:**
    1.  Client가 key-value 데이터를 저장 요청
    2.  `CRC16(key) mod 16384`로 hash slot 계산
    3.  해당 hash slot을 담당하는 **master node에만 저장**
    4.  Master가 background에서 해당 shard의 replica들에 비동기적으로 복제
  * **Read Operation:**
      * Master 또는 replica 둘 다에서 읽기 가능
      * 하지만 replica에서 읽으면 slightly stale data 가능성 있음

#### 장애 처리 (Failover)

  * Master B가 실패하면:
      * Replica B1이 자동으로 master로 승격
      * Hash slots 5501-11000을 B1이 담당하게 됨
  * 하지만 B와 B1이 동시에 실패하면 해당 hash slot 범위의 데이터에 접근 불가

#### 요약 (Summary)

당신의 이해가 거의 맞습니다\! 다만:

  * "3개의 DB"가 아니라 "**N개의 shard**"
  * 각 shard는 master 1개 + replica N개로 구성
  * Hash slot 기반으로 데이터 분산 (16,384개 slot)
  * Background replication은 맞습니다.

이해도가 좋네요. Redis Cluster의 핵심 아키텍처를 잘 파악하고 있습니다.

-----

### ||Q|| : Asking about Redis's key-value and dictionary-like data structure

그런데 redis db 는 무슨 키 밸류 ? 뭐시기로 저장한다고 들었는데 그게 nosql 이나 rdbms 와는 달라보이거든. 무슨 소리야? dictionary ? 라고 하던데.

### ||A|| : Explaining Redis as a key-value store with a dictionary structure

맞습니다\! Redis는 **key-value store**이고 **dictionary (해시 테이블)** 구조로 저장합니다.

#### Redis Key-Value Structure (Redis 키-값 구조)

  * **Basic Concept (기본 개념)**

      * Redis는 dictionary/hash table을 사용해서 데이터를 저장합니다.
      * `Key (unique string) → Value (any data type)`

#### RDBMS vs NoSQL vs Redis

| Database Type | 저장 방식 | 구조 |
| :--- | :--- | :--- |
| **RDBMS** | Tables with rows/columns | 관계형, 스키마 필요 |
| **NoSQL Document** | JSON-like documents | 문서 기반 |
| **Redis (Key-Value)** | Dictionary/Hash Table | 키-값 쌍 |

#### Redis Internal Dictionary (Redis 내부 딕셔너리)

  * Redis는 내부적으로 **2개의 hash table**을 가집니다:
      * `ht[0]`: 메인 해시 테이블
      * `ht[1]`: Rehashing 시에만 사용 (incremental rehashing)

#### Redis Value Types (Redis 값 타입들)

  * Key는 항상 string이지만, Value는 다양한 타입이 가능합니다:
      * **String:** `"hello world"`
      * **List:** `["a", "b", "c"]`
      * **Set:** `{a, b, c}`
      * **Hash:** `{name: "john", age: 30}`
      * **Sorted Set:** Scored elements
      * **Bitmap, JSON** 등

#### 실제 예시 (Practical Example)

```
SET user:1001:name "김철수"     # String value
HSET user:1001 name "김철수" age 25   # Hash value
LPUSH queue:jobs "job1" "job2"        # List value
```

#### 왜 Dictionary인가? (Why Dictionary?)

  * **O(1) lookup time:** 키로 바로 접근 가능
  * **Memory-based:** RAM에서 직접 처리로 초고속
  * **Simple but powerful:** 복잡한 쿼리 없이 키만으로 데이터 접근

정리: Redis = In-memory dictionary/hash table + 다양한 value types를 지원하는 NoSQL key-value store입니다. 테이블이나 문서가 아닌 **"키 → 값"** 매핑 구조죠.

-----

### Questions for Review

1.  Redis Cluster에서 Sharding과 Replication을 함께 사용하기 위한 최소 노드 수는 몇 개이며, 그 이유는 무엇인가요?
2.  Redis는 데이터를 분산시키기 위해 키(key)를 어떻게 특정 샤드(shard)에 할당하나요? (16,384라는 숫자와 관련된 개념을 설명하세요.)
3.  Redis의 복제(Replication) 과정에서 '전체 동기화(Full Synchronization)'와 '부분 동기화(Partial Synchronization)'는 어떤 차이가 있으며, 각각 언제 발생하나요?
4.  Redis Cluster 구성 요소를 설명할 때 '데이터베이스' 대신 '샤드(shard)' 또는 '노드(node)'라는 용어를 사용하는 것이 더 정확한 이유는 무엇인가요?
5.  Redis의 데이터 저장 방식이 전통적인 RDBMS와 어떻게 다른지 '구조'와 '접근 방식'의 관점에서 설명하세요.
6.  Redis의 키(Key)에 저장할 수 있는 값(Value)의 데이터 타입 3가지 이상을 예시와 함께 설명하세요.