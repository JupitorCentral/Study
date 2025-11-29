---
tags:
  - Java/Syntax
---

## BNF (Backus-Naur Form) in JLS

Java Language Specification (JLS)에서 문법을 정의할 때 사용하는 표기법.

### 표기법 의미

- **`{x}`**: x가 0번 이상 반복됨.
- **`[x]`**: x가 0번 또는 1번 나타남 (Optional).
- **`(one of)`**: 나열된 것 중 하나.

### 예시: Reference Type

```java
ReferenceType:
    ClassOrInterfaceType
    TypeVariable
    ArrayType
```

#### ClassOrInterfaceType
클래스나 인터페이스 타입.
`ClassType` 또는 `InterfaceType`.

```java
ClassType:
    {Annotation} Identifier [TypeArguments]
    ClassOrInterfaceType . {Annotation} Identifier [TypeArguments]
```
- 어노테이션이 붙을 수 있음.
- 제네릭 타입 인자(`[TypeArguments]`)가 올 수 있음.
- 내부 클래스는 `.`으로 연결됨.

#### ArrayType
배열 타입.
`Type Dims` 형태.

```java
Dims:
    {Annotation} [ ] {{Annotation} [ ]}
```
- `[]`가 반복됨 (다차원 배열).
- 각 차원마다 어노테이션을 붙일 수 있음 (예: `int @NonNull []`).
