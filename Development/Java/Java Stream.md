---
tags:
  - Java/Stream
---

## Stream

`map.entrySet()` 의 return type 은 Set\<Map.Entry\<K, V>> 이다.
즉 `Set` 을 반환한다. 
그러니까 `map.entrySet()` 은 `Set` 이고, `Set` 은 `Collection` 의 자식 클래스이므로 stream 을 호출 할 수 있다.
stream funtion 은 return type 이 Stream\<E> 이다.

### Stream ?

```java
public interface Stream<T> extends BaseStream<T, Stream<T>>
```

선언이 신기하다. 자기 자신이 들어가네 ?

#### F-bounded Polymorphsim or Recursive Generics
(F-제한 다형성 또는 재귀 제너릭)

-> 250925 

#### Function Class ?

```java
@FunctionalInterface  
public interface Function<T, R> { ... }
```
