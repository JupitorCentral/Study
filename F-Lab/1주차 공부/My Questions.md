
####  [[Spaces/Study/F-Lab/1주차 공부/250906.md#^a7bfed|synchornize]]

->  [[Spaces/Study/F-Lab/1주차 공부/250907.md#^65dc01|answer]]


####  variable++ / variable-- 와, ++variable / --variable 의 차이

##### 내가 아는 정보
 후위증감연산자 -> 해당 statement 가 끝나고 나면 해당 variable 의 값이 증가.
즉, 계산시 값이 증가하기 전의 값이 계산된 후에야 값이 증가된다.

전위 증감 연산자는 그 반대로, 해당 statement 가 끝나기 전 값이 증가한 후에 계산이 들어감.

##### 정답
statement 실행시, 값이 변하는 시점은 같다. 
그러나, 값이 변하기 전 값을 넘겨주느냐, 값이 변한 후의 값을 넘겨주냐의 차이 
(which value is returned at the expression)


#### 상속과 class variable

[[Spaces/Study/F-Lab/1주차 공부/250908.md#^6801f4|problem]]

이게 뭔소리지 ?

A 클래스가 있고 A 클래스를 상속받은 B 클래스가 있다.
A 클래스에는 static 변수 a 가 있다.
이때 B 클래스와 A 클래스가 메모리에 로딩될때, 변수 a 를 공유하는가 ?

```java
class ParentClass {  
    static int staticVariable = 5;  
  
    public static int getStaticVariable() {  
        return staticVariable;  
    }  
  
    public static void setStaticVariable(int staticVariable) {  
        ParentClass.staticVariable = staticVariable;  
    }  
}  
  
class ChildClass extends ParentClass {  
    public static int getStaticVariable() {  
        return staticVariable;  
    }  
  
    public static void setStaticVariable(int staticVariable) {  
        ParentClass.staticVariable = staticVariable;  
    }  
}  
  
public static void main(String[] args) throws Exception {  
    println("staticVariable from Parent : " + ParentClass.getStaticVariable());  
  
    ChildClass.setStaticVariable(6);  
    println("staticVariable from Child : " + ChildClass.getStaticVariable());  
    println("staticVariable from Parent : " + ParentClass.getStaticVariable());  
  
}
-----------
> Task :Main.main()
staticVariable from Parent : 5
staticVariable from Child : 6
staticVariable from Parent : 6
```

참고로 static variable, method 를 클래스의 static method 에서 호출할때
```java
this.staticVariable;
```
이런식으로 호출이 안되는데, 이는 'this' 가 instance reference 이기 때문이다.
-> 즉 객체가 생성되어야 사용될 수 있는 키워드가 this

그래서 ChildClass 에서도 this.staticVariable 이 안된다. 



#### 객체에 대해 == 은 같은 '주소' 를 의미하는지. 

[[Spaces/Study/F-Lab/1주차 공부/250909.md#^d70982|constant pool]]

변수 명이 다른데, 과연 같은 객체라고 할 수 있는지.
String s1 -> 여기서 s1 이라는 변수는 포인터라고 할 수 있는 것이 아닌지.
확인이 필요함. 


#### 왜 StringBuffer 는 Thread safe 하고, StringBuilder 는 그렇지 않은가 ?




#### return type 이 interface 인데 출력하는 경우

[[Spaces/Study/F-Lab/1주차 공부/250909.md#^3021fc|problem]]



#### bsil fd



#### 2의 보수, 1의 보수