---
tags:
  - Java/Generics
---

## Java Generics

### Type Parameter vs Type Argument

- **Type Parameter**: 제네릭 클래스나 메서드 선언부에 사용되는 식별자 (예: `<T>`, `<E>`).
    - 실제 타입이 아니라 나중에 구체적인 타입으로 대체될 **Placeholder**.
    - Scope: 클래스/인터페이스 전체, 메서드 내부.
    - **Static** 멤버에는 사용 불가 (인스턴스화 되기 전에 메모리에 올라가므로).

- **Type Argument**: 제네릭 타입을 사용할 때 실제로 전달되는 구체적인 타입 (예: `String`, `Integer`).

### Type Erasure (타입 소거)

Java의 제네릭은 컴파일 타임에만 존재하고, 런타임에는 **소거(Erasure)**된다.
즉, `List<String>`이나 `List<Integer>`나 런타임에는 모두 `List` (Raw Type)로 취급된다.

- **이유**: Java 5 이전 버전과의 하위 호환성을 위해.
- **작동 방식**:
    - `List<T>` -> `List`
    - `T extends Number` -> `Number` (Bound가 있으면 Bound로, 없으면 Object로 치환)
- **제약**: `new T()`, `new T[]` 등이 불가능한 이유 (런타임에 T가 뭔지 모르므로).

### Wildcard (?)

제네릭의 유연성을 높이기 위해 사용.

- **Unbounded Wildcard (`<?>`)**: 모든 타입 허용. 타입에 의존하지 않는 메서드(예: `size()`, `clear()`) 사용 시 적합.
- **Upper Bounded Wildcard (`<? extends T>`)**: T 또는 T의 하위 타입. (읽기 전용 - Producer)
- **Lower Bounded Wildcard (`<? super T>`)**: T 또는 T의 상위 타입. (쓰기 전용 - Consumer)

#### PECS (Producer-Extends, Consumer-Super)

- **Producer**: 데이터를 생산(제공)하는 객체라면 `extends`를 써라. (`get()` 가능, `add()` 불가)
    - 예: `List<? extends Number> numbers`. 여기서 값을 꺼내면 무조건 `Number`임이 보장됨.
- **Consumer**: 데이터를 소비(저장)하는 객체라면 `super`를 써라. (`add()` 가능, `get()`은 `Object`로만 가능)
    - 예: `List<? super Integer> list`. 여기에 `Integer`를 넣는 것은 안전함.

### Subtyping in Generics

- 배열은 **Covariant (공변)**: `Integer[]`는 `Number[]`의 하위 타입이다.
- 제네릭은 **Invariant (불변)**: `List<Integer>`는 `List<Number>`의 하위 타입이 **아니다**.
- 와일드카드를 사용하면 제네릭에서도 공변/반공변성을 흉내낼 수 있다.

### @SafeVarargs

제네릭과 가변 인자(Varargs)를 함께 사용할 때 발생하는 `Heap Pollution` 경고를 억제하는 어노테이션.
- 메서드가 타입 안전함을 보장할 때 사용 (배열에 값을 저장하지 않고, 배열 참조를 외부로 노출하지 않을 때).
