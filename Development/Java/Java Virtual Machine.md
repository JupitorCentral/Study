---
tags:
  - Java/JVM
---
# Java Virtual Machine (JVM)

## 1. JVM Basics
- **JVM**: 자바 바이트코드를 실행하는 가상 머신. OS에 종속적이지 않음 (Write Once, Run Anywhere).
- **JRE**: JVM + Class Libraries. 자바 프로그램 실행 환경.
- **JDK**: JRE + Development Tools (javac, etc.). 자바 개발 키트.

## 2. Java Compilation & Execution
### JIT (Just-In-Time) Compiler
- **인터프리터 방식**: 바이트코드를 한 줄씩 해석하여 실행. 초기 시작 속도는 빠르지만 반복 실행 시 느림.
- **JIT 컴파일러**: 자주 실행되는 코드(HotSpot)를 런타임에 네이티브 기계어로 컴파일하여 캐싱. 이후 실행 시 기계어를 바로 실행하여 성능 향상.
- **C/C++ 컴파일과의 차이**: C/C++은 실행 전(AOT)에 기계어로 번역하지만, 자바는 실행 중(Runtime)에 번역 및 최적화(PGO - Profile Guided Optimization)를 수행.

### HotSpot JVM
- Oracle/OpenJDK의 기본 JVM 구현체.
- **Tiered Compilation**:
    - **Interpreter**: 초기 실행. 프로파일링 정보 수집.
    - **C1 Compiler (Client)**: 빠른 컴파일, 간단한 최적화.
    - **C2 Compiler (Server)**: 느린 컴파일, 고도화된 최적화 (Inlining, Loop Unrolling 등).
- **Deoptimization**: 최적화된 가정이 틀렸을 때(예: 상속으로 인한 다형성 발생), 다시 인터프리터 모드로 돌아가는 기능.

## 3. JVM Memory Structure
### Metaspace (Java 8+)
- Java 7까지의 **PermGen**을 대체.
- **Native Memory** 영역에 저장됨 (Heap 아님).
- **저장 내용**:
    - 클래스 메타데이터 (Class Metadata)
    - 런타임 상수 풀 (Runtime Constant Pool)
    - 메서드 데이터 (Method Data)
- **장점**: OS가 관리하는 메모리를 사용하므로 힙 사이즈 제한으로부터 자유로움 (설정에 따라 자동 확장). OOME(PermGen Space) 오류 감소.

### Static Field Storage
- Java 8 이후:
    - **Static Variables**: Heap 영역의 Class Object에 저장됨.
    - **Interned Strings**: Heap 영역의 String Pool에 저장됨.
    - **Class Metadata**: Metaspace에 저장됨.
