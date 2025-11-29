---
tags:
  - Java/GC
---

## GC Concepts

#### GC (Garbage collector)
자바는 불필요한 자원(객체)을 알아서 처리해준다.

#### Process of GC (Generational GC)
- **Young Generation**: Eden, Survivor 0, Survivor 1.
    - 객체는 Eden에 생성. Minor GC 발생 시 살아남은 객체는 Survivor로 이동.
    - Survivor 영역 중 하나는 항상 비어 있어야 함.
- **Old Generation**: Young에서 오랫동안 살아남은 객체가 이동(Promotion).
    - Major GC(Full GC) 발생 대상.
- **Metaspace (Java 8+)**: 클래스 메타데이터 저장. Native Memory 사용.

#### GC Algorithms
- **Serial GC**: 단일 스레드.
- **Parallel GC (Throughput)**: 멀티 스레드 Minor GC.
- **CMS GC**: Low Latency 지향. Compaction 없음.
- **G1 GC**: Region 단위 관리. 대용량 힙에 적합.

### Minor GC vs Full GC
- **Minor GC**: Young 영역 대상. 빠름. 대부분의 객체는 금방 죽기 때문에 효율적.
- **Full GC**: Old 영역 대상. 느림. 전체 힙을 정리하고 Compaction(압축) 수행. Stop-The-World 시간이 김.

## GC Deep Dive

### Mark and Sweep
GC의 기본 알고리즘.
1. **Mark**: GC Root로부터 도달 가능한(Live) 객체를 식별하여 마킹.
2. **Sweep**: 마킹되지 않은 객체(Garbage)를 힙에서 제거.
단점: 메모리 단편화(Fragmentation) 발생 가능 -> Compaction으로 해결.

### GC Root
힙 외부에서 힙 내부의 객체를 가리키는 참조들.
- Stack Frame의 Local Variables (지역 변수)
- Method Area의 Static Variables (정적 변수)
- JNI References (Native Method 참조)
- Registers

### Card Table
Old Generation 객체가 Young Generation 객체를 참조할 때, Minor GC 시 Old 영역 전체를 스캔하지 않기 위해 사용하는 구조.
- Old 영역을 512바이트 청크(Card)로 나눔.
- 참조가 발생하면 해당 카드를 Dirty로 표시.
- Minor GC 시 Dirty Card만 스캔하여 GC Root로 사용.

## Reference Types (Target of GC)

객체의 참조 강도에 따라 GC 대상 여부가 달라짐.

| Reference Type | GC 대상 여부 (Eligibility) | 설명 |
| :--- | :--- | :--- |
| **Strong Reference** | GC 대상 아님 | 일반적인 `new` 할당 (`Object o = new Object()`). 참조가 있는 한 수거 안 됨. |
| **Soft Reference** | 메모리 부족 시 대상 | OOME 발생 직전에 수거됨. 캐싱에 주로 사용. |
| **Weak Reference** | 즉시 대상 | Strong Reference가 없고 Weak Reference만 남으면 다음 GC 때 무조건 수거. (`WeakHashMap`, `ThreadLocal` 등에서 사용) |
| **Phantom Reference** | 수거 직전 | 객체가 메모리에서 해제된 후, 파이널라이즈 작업 등을 위해 사용. |
