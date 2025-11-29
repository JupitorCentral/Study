```java
  
import java.util.Arrays;  
  
  
public class Solution {  
  
    public int longestConsecutive(int[] nums) {  
          
        int max = 0;  
        int i = 0;  
        int current = 0;  
        int previous = -1;  
  
        Arrays.sort(nums);  
  
        while (i < nums.length) {  
            if (i==0) {  
                max = 1;  
                current = 1;  
            } else {  
                if (nums[i] == previous) {}  
                else if (nums[i] == previous+1) {  
                    current++;  
                    max = Math.max(max, current);  
                } else {  
                    current = 1;  
                }  
            }  
            previous = nums[i];  
            i++;  
        }  
        return max;  
    }  
}
```

수가 중복되는 경우를 패스하는 경우를 생각했었어야 했었다.
근데 이때 continue 를 보내버리면 또 안된다. 
밑에 i++ 가 안되기때문에...

차라리 이럴거면 for문을 썼어도 됬을 것을...흠


