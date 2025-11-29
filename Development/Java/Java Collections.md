---
tags:
  - Java/Collections
---

## 자바 자료구조 크게 4가지

- list
- queue
- map
- set


## ArrayList

다른 컬렉션 공부를 다 하면 1권을 못나가니까 일단 질문에 대해서만.

>  질문 : Arraylist 가 꽉 차면 새로운 공간을 만들고 기존 값을 복사한다. 이럴때의 Big O 는 ?

ArrayList 가 무엇인가


ArrayList 의 메소드들은 무엇이 있는가 -> 일단 Object 로 가자 왜나하면 모든 클래스들은 Object 를 최상단으로 상속받음.

ArrayList 가 꽉차면 어떤 일이 벌어지는가


## List<Integer> arr <- 이 arr 에 null 을 add 할 수 있을까 ? 


```java
public static void main(String[] args) throws Exception {  
  
    ArrayList<Integer> arr = new ArrayList<>();  
  
    arr.add( 3 );  
    arr.add( 5 );  
    arr.add(null);  
      
    for (Integer x : arr) {  
        print(x);
    }  
}
```

분명, 위 내용으로 봤을때, ArrayList 에 add 가 추가되는것은, reference 가 추가되는 것일 것이다.
그렇지 않으면 null 이 될 수가 없다.

근데 primitive type 은 어떨지 궁금하다.
내부적으로 3 의 값을 가지는 Integer object 를 생성하고 그를 가리키는 reference 를 추가하는게 아닐지 싶다.
그러니까 arr.add ( 3 ) 은 arr.add ( Integer.valueOf( 3 ) ) 이 아닐지?

(intever.valueOf 도 코드를 보면 새로 생성해서 리턴해준다. Effective Java 에 나온 이름이 있는 Constructor Factory 패턴.)


# 자바의 신 2권

## Collection

List -> 순서가 있는 목록
Set -> 순서가 중요하지 않는 목록
Queue -> 먼저 들어온 것이 먼저 나가는 목록
Map -> key-value 

![[Screenshot 2025-09-21 at 1.55.14 AM.png|500]]

![[Screenshot 2025-09-21 at 1.57.45 AM.png|400]]

점선은 interface,  실선은 클래스이다.


### Collection interface

publid interface Collection<E> extends Iterable<E>

Iterator() -> Iterater <T> 반환 
add -> 요소 추가
addAll (Collection) -> 매개변수 컬렉션의 모든 요소 추가
clear -> 전부 삭제
contains (Object) -> 콜렉션에 해당 객체가 존재하는지
containsAll (Collection) -> 매개변수 컬렉션의 모든 요소가 콜렉션 에 다 존재하는지
hashCode -> 해쉬코드 리턴

isEmpty
remove
removeAll (Collection) -> 매개변수로 넘어온 Collection 에 해당하는 모든 객체를 콜렉션에서 삭제
boolean retainAll (collection) -> 매개변수로 넘어온 Collection 만을 컬렉션에 남기는 것
int size()
Object[]  toArray
<T> T[] toArray(T[]) -> 컬렉션에 있는 데이터들을 지정한 타입의 배열로 복사

### ArrayList - implementing List Interface 

ArrayList. Vector, Stack, LinkedList 등

ArrayList, Vector -> 비슷함. 둘 다  '확장 가능한 배열'. 대부분의 메소드가 동일함.
차이점 : Thread Safe (Vector가 ThreadSafe 하다)

Stack -> Vector 를 확장 -> LIFO 를 위해서

#### Implemented Interfaces

Serializable -> 원격으로 객체를 전송하거나, 파일에 저장할 수 있음을 지정 (실제로 구현해야 하는 메소드가 없음)
Cloneable -> Object 클래스의 clone() 메소드가 제대로 수행될 수 있음을 지정. 복제가 가능한 객체임을 의미.

##### Iterable<E>

###### ==forEach==

-> "forEach"  문장을 사용할 수 있음. forEach 뿐만 아니라 SpliterRator 라는 것도 있음. 이게 뭐지...
-> Lambda Expression

```java
default void forEach(Consumer<? super T> action) {  
    Objects.requireNonNull(action);  
    for (T t : this) {  
        action.accept(t);  
    }  
}
```

##### Collection 
-> 여러개의 객체를 하나의 객체를 담아 처리할때의 메소드 지정

##### List 
-> 목록형 데이터를 처리하는 것과 관련된 메소드 지정

##### RandomAccess

임의 접근 알고리즘이 적용된다는 의미

#### Constructor

3가지 -> empty, 다른 컬렉션으로 만들기, 특정 capacity

```java
ArrayList<String> strings = new ArrayList<>();
```

-> jdk 7 부터는 생성자를 호출하는 부분에 따로 탕비을 적지 않고 <> 로만 사용해도 됨.

##### Capacity

-> Capacity 를 초과하게 되면 이를 늘리는 작업을 하게되고, 이는 성능에 부담을 준다.
-> 따라서 초기 Capacity 를 잘 정하는 것도 중요함

-> ArrayList 에서 크기를 늘리는 작업의 시간복잡도는 O ( 1 ) 로 들었었다. 


#### Adding to ArrayList

add (E e)
add (int index, E e)
addAll (Collection<? extends E> c)
addAll( int index, Collection<? extends E> c)

Collection 에 선언된 구조체를 추가할 수 있다는 것을 명심


#### Retrieve from ArrayList

배열 -> 배열.length
ArrayList -> size()

근데 둘은 같은 의미가 아니다. 배열의 length 는 공간 크기
ArrayList 의 size() 는 공간 크기가 아니라 들어가 있는 값의 개수이다. ( != Capacity )

E get (index i)
int indexOf (Object o) -> 매개변수 객체와 동일한 객체의 위치 리턴
int lastIndexOf (Object o) -> 매개변수 객체와 동일한 객체의 위치 리턴 -> 끝에서부터 탐색


##### 배열로 뽑아내고 싶다면

Object[] toArray
-> Object 의 배열로만 리턴해버린다

<T> T[] toArray(T[] a)


## Map 

map.entrySet ? 
stream ?
sorted ?
Map.Entry<Integer, Integer>comparingByValue ? 

### Map.entrySet

```java
public static void main(String[] args) throws Exception {  
  
    Map<Integer, Integer> map = new HashMap<>();  
  
    map.put(3, 4);  
    map.put(5, 3);  
  
    for (Map.Entry<Integer, Integer> entry : map.entrySet() ) {  
        entry.setValue(15);
    }  
  
    for (Map.Entry<Integer, Integer> entry : map.entrySet() ) {  
        printf("key : " + entry.getKey() + ", value : " + entry.getValue() + "\n");  
    }   
}
-------
key : 3, value : 15
key : 5, value : 15

```

javaDoc 을 보면 entrySet 은 Map.Entry 형식으로 key-value pair 를 반환하는데,
이 Entry 들은 원래 Map 을 투영하며, 연결을 유지한다고 함. (These instances maintain a connection to the original, backing map.)
for 문 안에서 entry 는 entry-set view 의 반복 동안에만 그 연결을 유지함.

연결이 유지되면, 어떤 Entrry 의 값을 수정하면 원본 Map 에 영향이 간다는 이야기.
그러니까 쉽게말하자면 map 의 iteration 안의 entry 를 수정하면 map 의 해당 키-value 값에 영향이 간다는 뜻.

해당 Map 의 iteration 외부에서의 Entry 에 대한 접근은 undefined, 예측할 수 없다.
-> 내 생각에는 외부에 Entry reference 를 만들고 iteration  내부에서 해당 reference 에 특정 entry 를 집어넣을 경우를 말하는 듯 하다.

```