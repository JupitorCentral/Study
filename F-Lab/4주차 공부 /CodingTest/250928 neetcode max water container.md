
```java
public class Solution {  
    public int maxArea(int[] heights) {  
       
        int i=0;   
        int j = heights.length-1;  
        int max = Integer.MIN_VALUE;  
  
        while (i < j) {   
            max = Math.max(calculateArea(heights, i, j), max);  
            if (heights[i] > heights[j]) j--;  
            else i++;  
        }  
        return max;  
    }  
  
    int calculateArea (int[] heights, int i, int j) {  
        return  Math.min(heights[i], heights[j]) * Math.abs(i-j);  
    }  
}
```

힌트를 보고 풀긴 풀었는데, 왜 작은 height 을 가진 index 만 옮겨야 하는지 모르겠다.
다른 경우의 수가 없어서 인가? 그것 외에 ?

https://neetcode.io/problems/max-water-container?list=neetcode150

설명을 한번 봐야겠다.
