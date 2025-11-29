brute force, two pointer, dynamic programming 으로 풀기 가능


### Two Pointer


```java
public int maxProfit(int[] prices) {  
    int l=0;  
    int r=1;  
    int max = 0;  
  
    int current;  
    while (r < prices.length) {  
        int left = prices[l];  
        int right = prices[r];  
  
        if (right >= left) {  
            current = right - left;  
            if (current > max) {  
                max = current;  
            }  
        } else {  
            l = r;  
        }  
        r++;  
    }  
    return max;  
}
```

Time Complexity : O(n), Space Complexity O( 1 )

포인터 2개를 가지고
왼쪽은 살때의 값, 오른쪽은 팔떄의 값이다.

이것도 right 포인터를 일일이 옮겨가면서, 저점을 고정하고 right 포인터의 값을 이용해 이윤을 측정한 뒤에
max value 와 비교한다.


### Dynamic programming

```java
public int maxProfit(int[] prices) {  
    int max = 0;  
    int minBuy = prices[0];  
  
    for (int p : prices) {  
        max = Math.max(max, p - minBuy);  
        minBuy = Math.min(p, minBuy);  
    }  
  
    return max;  
}
```

Time Complexity : O(n), Space Complexity O( 1 )

어떤 한 지점에 대해서, 기존의 max 값과, 새로 계산된 max 값 (현지점 - 이전의 가장 저점) 을 비교한다.
그러면 max 가 항상 max 로 유지됨.

최솟값을 항상 업데이트.

이게 말로 설명은 참 어려운데, 한번 동작하는 매커니즘을 보니 이해가 간다.

내가 생각한 반례중 5 8 1 7 이렇게 있을 수 있는데
잘 보면 5 일때는 minBuy = 5, max = max (0, p - minBuy = 0)
8 : max = max (0, 8 - 5 = 3). minBuy = 5
1 : max = 3, minbuy = 1
7 : max = max (3, 7-1(minBuy) = 6) = 6, minBuy = 1 (유지)

이게 개념만 잘 익히면 훨씬 쉬운데, 적용하기가 어렵다.

방향이 중요한 것 같다.
