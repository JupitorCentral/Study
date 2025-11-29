--- 
tags:
  - CS/Architecture
  - CS/Memory
---

#### 메모리와 하드디스크의 차이점은 ?


#### Primitive Type, reference Type 이 저장되는 공간은 어디인가 ?


#### 자바에서, stack 에는 뭐가들어가고 heap 에는 뭐가 들어가는가 ?


#### 상황별 data passing 속도

``` 
- L1 cache reference 0.5 ns 
- Branch mispredict 5 ns 
- L2 cache reference 7 ns 
- Mutex lock/unlock 100 ns 
- Main memory reference 100 ns 
- Compress 1K bytes with Zippy 10,000 ns 
- Send 2K bytes over 1 Gbps network 20,000 ns 
- Read 1 MB sequentially from memory 250,000 ns 
- Round trip within same datacenter 500,000 ns 
- Disk seek 10,000,000 ns 
- Read 1 MB sequentially from network 10,000,000 ns 
- Read 1 MB sequentially from disk 30,000,000 ns 
- Send packet CA->Netherlands->CA 150,000,000 ns
```



#### 디스크와 메모리의 차이점은 ?

디스크는 영구저장, 메모리는 휘발성
디스크는 access 속도가 메모리에 비해 느리다.

-> 그렇다면 느린 이유는 ?
메모리는 랜덤엑세스이고, 디스크는 데이터를 찾기 위해 실린더가 물리적으로 돌아가기 때문.

-> 메모리가 random access 라는 부분에 대해서 더 공부해야 할듯.

#### random access 는 무엇이며, 그 와 비교될 다른것은 ?


#### 2의 보수 이해 -> 부동 소수점 이해

0.9999 = 1 이 되는 이슈들을 해결하지 못하면 난리가 남.
돈과 관련된건 무조건 BigDecimal 을 써야 하는 이유.

1의 보수도 있는데 왜 2의 보수를 사용하는가 ?


2의 보수를 위한 멘토님 블로그 글 참조.

[https://blog.yevgnenll.me/posts/why-computer-use-twos-complements-calculate-minus](https://blog.yevgnenll.me/posts/why-computer-use-twos-complements-calculate-minus)


## 2의 보수, 1의 보수 이해 -> 왜 2의 보수를 사용하는가 ?

컴퓨터에서 수를 표현하는 방법 2가지 -> 1의 보수, 2의 보수

### 1의 보수

0101 의 음수를 구하려면, 1010. 비트를 반전시킨다.

#### pros
비트만 반전시키면 되니까 음수를 구하기 쉽다.

#### cons

1111 -> -0, 0000 -> 0 즉 0 이 두가지이다.
-0 에 대해 문제가 생기는데 

첫번째, 소프트웨어  0 을 표현할때 뭘로 표현할지 애매해진다. 0000 으로 할지, 1111로 할지 ?
그리고, 값이 1111 또는 0000 일때 모두 0으로 취급해야 하므로, 두번 체크 해야 한다. -> 즉 직관적이지 못함.

end-around carry 가 발생하면 1을 더해주어야 한다.

 3 + (-0) 을 더해보면
   0 0 1 1
   1  1  1 1 (-0)
1 0 0  1 0  -> +2 가 된다.

이를 위해선 1을 더해주어야 한다.

6 - 19
   0000 0 1  1  0   ( 6 )
   1 1 1 0  1  1  0 0  ( - 19 )
   1  1 1 1  0 0 1  0 
   -> 12 가 된다.

![[Screenshot 2025-09-16 at 10.06.29 PM.png|400]]

-> 즉, 연산이 복잡해진다.

그리고 2의 보수에 비해서 음수 1개가 빈다. 즉 표현할 수 있는 수가 한개 줄어든다.



### 2의 보수

음수를 구하기 위해서, 비트를 모두 반전하고 거기에 1을 더한다.

 0 0 1 0   -> 2
 1  1  0 1
 1  1  1  0    -> -2

( - 0 ) 의 경우

0 0 0 0
1  1  1  1
0 0 0 0   -> - 0 이 없음

그럼 최댓값은 ?

0 1 1 1   -> 7
1 0 0 0 
1 0 0 1  -> -7   -> 여전히 하나가 빈다.

1  1 1 1   -> 0 0 0 1  ->  1 1 1 1 은 -1
1  1 1 0   ->  0 0 1 0  -> 1 1 1 0 은 -2
이대로 가면
1 0 0 0   ->  8 개  ( 1 1 1 1 ~ 1 0 0 0 ) 에서 0 0 0 ~ 1 1 1 은 8개다.
즉 1 0 0 0 은 - 8 이라고 생각할 수 있다.
0 1 1 1 은 7.


그럼 위 방법으로 위의 경우를 계산해보면...
( - 0 ) 에 대한 경우에는 계산할 필요가 없으니

  0 0 0 0   0  1   1  0   ( 6 )
   1  1  1  0   1   1   0  1  ( - 19 )
   1  1  1   1    0  0  1   1 

음수니까 반전 + 1 하면

0 0 0 0  1 1 0 1   -> 13  -> 바로 딱 떨어진다

1 의 보수와 다르게 캐리 무시 가능.



## 부동 소수점 이해

^b79a6d

floating 포인트는 2의 보수를 쓰지 않는다.


4.5 를 IEEE 754 single precision 으로 구해보면

![[Screenshot 2025-09-16 at 10.43.37 PM.png|600]]

![[Screenshot 2025-09-16 at 10.43.58 PM.png|600]]


즉, 4.5 를 100.1 로 먼저 표현. (이진수로 표현.) 4 -> 100, 0.5 -> 2분의 1 -> 2^(-1) -> 0.1
-> normalization 1.001 e^2 . 즉 지수 : 2, 가수 : 001 (1은 무조건 있다고 가정. 0을 표현하면, 가수 23비트는 모두 0이 되므로 표현 가능)

지수 2에 대해서, bias 127 을 사용, 127 을 지수에 더해서 129 를 2진수로 표현

즉 가수에 대한 2진수 구하기 + 지수 구하기 + 지수에 bias 더해서 이진수로 표현하기
signed bit + base ^ exponent + Mantissa (Significand)


### 0.1 + 0.2 != 0.3

위에 표현에 따르면, 0.5 = 2^-1. 0.25 = 2^-2. 그러니까, 소수점 아래 수는 2의 나누기로 표현한다.
이때, 0.1 과 0.2 는 이진수의 소수점 아래 표현으로 무한소수로밖에 표현이 안된다.
(0.2 가 문제인듯. 0.1 -> 0.2 가 되니까)

![[Screenshot 2025-09-17 at 2.20.50 AM.png|300]]

IEEE 754 의 정밀도의 한계는, 
Single Precision 의 경우 23bit -> 유효 자릿수 약 7자리
Double Precision 의 경우 52bit  -> 유효 자릿수 약 15-16 자리



![[Screenshot 2025-09-17 at 2.21.48 AM.png|600]]

그래서 0.1 + 0.2 != 0.3 인 것이다.
이를 보완하려면, 특정 자릿수에서 특정 오차 이내에는 값이 같다고 판정하게끔 하는 것으로 해결

```javascript
// 문제
0.1 + 0.2 === 0.3; // false

// 해결법 1: Number.EPSILON 사용
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON; // true

// 해결법 2: 정수로 변환 후 계산
(1 + 2) / 10 === 0.3; // true

// 해결법 3: toFixed() 사용
(0.1 + 0.2).toFixed(1) === '0.3'; // true
```


## 부동 소수점과 -0

[[250916#^b79a6d]] 에 이어서


부동 소수점은 부호 + 지수 + 가수 부분으로 나누어져 있다.
부호 가있기때문에 -0이 표현 가능하다.

-0을 가질 수 있는 1의 보수에서는 -0 때문에 하드웨어 구현 난이도의 증가, 불필요한 연산 등 안좋은 점이 많았지만
부동소수점의 -0은 유용한 기능을 가진다.

부동 소수점은 소수를 나타내기때문에, 아주 작은 수를 표현할 수 있다.
이는 근사치를 표현할 수 있다는 얘기.
즉 극한을 표현할 수 있는데
-0 이 표현 가능하다는 것은 그 극한을 완성하는, 즉 -inf 을 표현할 수 있는 근거가 된다.

이는 수치해석이나 물리 시물레이션 등에서 '방향성 정보'를 나타낼 수 있는 것이다.

### 왜 +0과 -0이 유용한가?


IEEE 754 표준에서는 +0 과 -0을 구분해서 저장하므로

  $\lim _{x	o 0+\:}\left(\frac{1}{x}\right)=+∞$ , $\lim _{x	o 0-\:}\left(\frac{1}{x}\right)=-∞$


 등의 0에 "어느 방향에서 접근했는지"를 코드로도 표현할 수 있음.

실제로,

$\frac{1}{+0}=+\infty$ , $\frac{1}{-0}=-\infty$

가 나오고, 이건 수치해석이나 물리 시뮬레이션 등에서 방향성 정보를 보존하는 데 매우 유 하다.

```python
// python
print(1.0 / 0.0)   # inf
print(1.0 / -0.0)  # -inf
```

```java
// java
System.out.println(1.0 / 0.0);   // Infinity
System.out.println(1.0 / -0.0);  // -Infinity

```

#### 마인크래프트에서

(0, 0) 좌표는 4개의 블록이 교접하는 공간이 된다.
이때, 0,0 에 캐릭터가 접근할때, 어떤 방향에서 접근하는지를 알기 위해서는 -0, +0 의 구분이 필요함.
어느쪽에서 접근했는지를 알아야 하기 때문.
그게 안된다면 정확한 위치 판정이 어렵다.

또한 블록의 표시에서

```javascript
function isNegZero(x) {
  return x === 0 && (1 / x === -Infinity);
}

function blockPosition(x, y) {
  // -0.0은 왼쪽/아래 블록, +0.0은 오른쪽/위 블록으로 판정
  const xSide = isNegZero(x) ? 'left' : 'right';
  const ySide = isNegZero(y) ? 'bottom' : 'top';
  return `${xSide} ${Math.abs(x)}, ${ySide} ${Math.abs(y)}`;
}

console.log(blockPosition(-0.0, 0.0)); // left 0, top 0
console.log(blockPosition(0.0, -0.0)); // right 0, bottom 0
```

즉 +0, -0 이 있어야 0,0 지점에서 맞물려있는 블록 4개를 정확하게 표현할 수 있음.
그렇지 않으면 버그 발생.



#### 수학 - 허수에서

```python
import cmath
z = complex(-1, 0.0)
print(cmath.sqrt(z))      # 0j + 1j

z_neg = complex(-1, -0.0)
print(cmath.sqrt(z_neg))  # 0j - 1j
```

```python
import cmath
z1 = complex(-2.0, 0.0)    # branch cut 위, 위에서 접근
z2 = complex(-2.0, -0.0)   # branch cut 위, 아래에서 접근

print(cmath.sqrt(z1))      # 1.4142135623730951j
print(cmath.sqrt(z2))      # -1.4142135623730951j
```


허수부의 −0.0−0.0이 실제로 결과의 방향을 결정한다.
-> 복소수의 제곱근을 몰라 이해 못했음.

```