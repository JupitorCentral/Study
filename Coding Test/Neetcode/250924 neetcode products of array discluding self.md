```java
public int[] productExceptSelf(int[] nums) {  
    int[] answer = new int[nums.length];  
    int[] s = new int[nums.length];  
    int[] e = new int[nums.length];  
  
    int current = 1;  
    for (int i=0; i<nums.length; i++) {  
        s[i] = current;  
        current *= nums[i];  
    }  
    current = 1;  
    for (int i=nums.length-1; i>=0; i--) {  
        e[i] = current;  
        current *= nums[i];  
    }  
  
    for (int i=0; i<nums.length; i++) {  
        answer[i] = s[i] * e[i];  
    }  
  
    return  answer;  
}
```

일종의 dynamic programming 인데, 처음에는 힌트를 보고 몰랐다가
아이패드에 힌트를 보면서 진행해보니 조금 이해는 갔다.

어떻게 해야하는지는 알겠는데 완전히 이해는 못한 케이스.
역시 수학에 약한 것인가...

아, 그러니까 왼쪽부터 오른쪽으로 갈때
모든 수를 곱해나가는데 이때
어떤 자리 i 에 대해서 prefix sum \[i] 는 i 이전의 모든 곱이 되고

suffix sum 을 구할때
어떤 i 에 대해서 suffix sum \[i] 는 i 다음의 모든 곱이 된다 (스스로를 빼고)

그러니까 prefix sum \[i] * suffix sum\[i] 는 1 * 2 * 3 ... (i 빼고) i+1 * i+2 *  ... * n

