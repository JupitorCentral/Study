```java
package JAVAPRACTICE17;  
  
public class Solution {  
    public int trap(int[] height) {  
        int answer = 0;  
  
        int[] leftMost = new int[height.length];  
        int[] rightMost = new int[height.length];  
  
        // water[i] = Math.max(Math.min(leftMost[i], rightMost[i]) - height[i]. 0)  
  
        // find leftMost        int max = height[0];  
        for (int i=1; i<height.length; i++) {  
            if (max <= height[i-1]) {   
                max = height[i-1];  
            }  
            leftMost[i] = max;  
        }  
  
        max = height[height.length-1];  
        for (int i=height.length-2; i>=0; i--) {  
            if (max < height[i+1]) {  
                max = height[i+1];  
            }  
            rightMost[i] = max;  
        }  
  
        for (int i=0; i<height.length; i++) {  
            answer += Math.max(Math.min(leftMost[i], rightMost[i]) - height[i], 0);  
        }  
          
        return answer;  
    }   
}
```

Hard 난이도인데 힌트 2까지만 보고 풀 수 있었다.

space complexity, time complexity 모두 O ( n ) 으로 조건에 만족한다.

맨 처음에는 포인터 2개를 가지고 2개의 테두리를 구하면서 하려고 했으나
테두리가 정해지는 시점에서 중간에 있는 지점들에 담긴 물의 양을 측정할 수가 없었다.

구하고 나서 그 안을 또 for loop 을 도는 것도 생각해봤지만, 너무 코드가 복잡해지는 기분이 들었다.

그러고 나서 힌트 2를 보자마자,
하나의 지점에서의 물의 양은 안에서 그 지점 왼쪽에 가장 큰 벽 그리고 오른쪽에서 가장 큰 벽의 크기
그리고 현재의 높이 이 3가지에 대해서만 영향을 받는다는 것을 알게되었다.

굿.



