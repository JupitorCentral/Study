```java
  
public class Solution {  
    public int[] twoSum(int[] numbers, int target) {  
  
        int[] answer = new int[2];  
        int i = 0;  
        int j = numbers.length-1;  
  
        while (true) {   
            int sum = numbers[i] + numbers[j];  
            if (sum == target) break;  
            if (sum < target) i++;  
            else j--;  
        }  
       
        answer[0] = i+1;  
        answer[1] = j+1;  
        return answer;  
    }  
}
```

사실 예전에 풀어봤던 문제긴하다.
increasing order 에서 양끝에 포인터를 두고
더한값이 크면 오른쪽의 포인트를 아래로 내리면 더한 값이 줄어들고
더한값이 작으면 왼쪽의 포인트를 올려서 더한값이 커지게 만들면 된다.

