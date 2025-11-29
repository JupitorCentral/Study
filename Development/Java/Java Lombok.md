---
tags:
  - Java/Library
  - Java/Lombok
---

## Lombok

자바의 Boilerplate code(getter, setter, 생성자 등)를 어노테이션으로 자동 생성해주는 라이브러리.

### 주요 어노테이션

#### @Getter / @Setter
- 필드의 Getter/Setter 메서드 자동 생성.
- **주의**: Entity에서는 Setter 사용을 지양하고, 비즈니스 메서드를 사용하는 것이 좋음.

#### 생성자 관련
- **@NoArgsConstructor**: 파라미터 없는 기본 생성자 생성.
    - **JPA Entity 필수** (Reflection 사용 시 필요). `access = AccessLevel.PROTECTED` 권장.
- **@AllArgsConstructor**: 모든 필드를 파라미터로 받는 생성자 생성.
- **@RequiredArgsConstructor**: `final` 필드나 `@NonNull` 필드만 받는 생성자 생성.
    - **Spring DI** (생성자 주입) 시 매우 유용.

#### @Data
- `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@ToString`, `@EqualsAndHashCode`를 한 번에 적용.
- **주의**: Entity에서는 사용 지양 (양방향 연관관계 시 `toString()` 무한 루프 등 문제 발생 가능).

#### @Builder
- Builder 패턴을 자동 구현.
- `@AllArgsConstructor`가 내부적으로 필요함.

#### @Slf4j
- 로깅 프레임워크(SLF4J)의 Logger 객체(`log`)를 자동 생성.
