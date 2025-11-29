---
tags:
  - CS/OS
  - CS/Process
  - CS/Thread
---

## Process vs Thread

### 1. 개념 (Concept)
- **Process (프로세스)**: 실행 중인 프로그램의 인스턴스. 자원(메모리, 파일 등) 할당의 단위.
- **Thread (스레드)**: 프로세스 내에서 실행되는 흐름의 단위. CPU 스케줄링의 단위.

### 2. 메모리 구조 & 자원 공유
- **Process**: 독립적인 메모리 공간(Code, Data, Heap, Stack)을 가짐. 다른 프로세스의 메모리에 직접 접근 불가.
    - **IPC (Inter-Process Communication)**: 프로세스 간 통신을 위해 파이프, 소켓, 공유 메모리 등이 필요.
- **Thread**:
    - **공유**: 프로세스의 Code, Data(전역변수), Heap(객체) 영역을 공유.
    - **독립**: 각 스레드마다 고유의 **Stack**과 **PC(Program Counter)**, **Register** 값을 가짐.

### 3. Concurrency vs Parallelism
- **Concurrency (동시성)**: 논리적 개념. 싱글 코어에서 여러 작업을 번갈아가며 실행(Context Switching)하여 동시에 실행되는 것처럼 보이게 함.
- **Parallelism (병렬성)**: 물리적 개념. 멀티 코어에서 실제로 여러 작업이 동시에 실행됨.

### 4. 선택 기준 (Selection Criteria)
- **Process 사용**:
    - **안정성/격리**: 한 작업의 크래시가 전체 시스템에 영향을 주지 않아야 할 때 (예: 브라우저 탭, Chrome 멀티 프로세스 아키텍처).
    - **보안**: 작업 간 메모리 격리가 중요할 때.
- **Thread 사용**:
    - **성능/효율**: 자원 생성/해제 비용과 컨텍스트 스위칭 오버헤드가 적어야 할 때.
    - **데이터 공유**: 작업 간 빈번한 데이터 교환이 필요할 때 (예: 웹 서버의 요청 처리).

### 5. Thread Pool
- 스레드 생성/소멸 비용을 줄이기 위해 미리 스레드를 만들어두고 재사용하는 기법.
- **Tomcat Thread Pool**: 웹 요청마다 스레드를 생성하는 오버헤드를 방지하기 위해 Worker Thread Pool을 운영.
    - `minSpareThreads`: 유휴 상태로 유지할 최소 스레드 수.
    - `maxThreads`: 최대 동시 처리 스레드 수.
