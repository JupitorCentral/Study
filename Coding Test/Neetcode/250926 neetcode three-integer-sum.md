```java
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.List;  
  
public class Solution {  
  
    List<List<Integer>> answer = new ArrayList<>();  
    List<Integer> sequence = new ArrayList<>();  
  
    public List<List<Integer>> threeSum(int[] nums) {  
          
        search(nums, 0);  
  
        return answer;  
    }  
  
    public void search (int[] nums, int level) {  
        if  (3 == sequence.size()) {  
            List<Integer> temp = new ArrayList<>(sequence);  
            Collections.sort(temp);  
            if (checkSum() && !answer.contains(temp)) answer.add(temp);  
            return;  
        }  
  
        for (int i=level; i<nums.length; i++) {  
            sequence.add(nums[i]);  
            search(nums, i+1);  
            sequence.remove(sequence.size()-1);  
        }  
    }  
  
    public boolean  checkSum () {  
        int sum = 0;  
        for (Integer x : sequence) {  
            sum += x;  
        }  
        return 0 == sum;  
    }  
}
```

backtracking 이용하여 nums 의 모든 조합을 구하여 brute force 하였다.

처음에는 \[ \[] \[] \[] ] 으로 나와서 당황했으나,
이내 그냥 sequence 를 answer 에 대입만 해버리면
동일한 객체가 answer 에 추가가 되기때문에 
backtracking 의 종료시에 sequence 가 비어버리는 일이 생겨 그런 일이 발생한다는 것을 알아챌 수 있었다.

그리고 answer 에 sequence 가 중복이 되어 살펴보았더니
입력값에 중복이 있었고,

어차피 객체를 새로 생성해야 하기 때문에 answer 에 추가하기전에 Collections.sort 를 이용하여 정렬하고
이 객체가 기존에 answer 에 이미 있었는지 판단 후에 추가하게 만들었다.

문제에서는 답의 '순서' 가 어찌됬든 상관이 없다고 했는데,
그럼 처음부터 nums 에 중복을 제거하면 되지 않는가 ? 라고 생각해서 다시 풀어보았다.

이 과정에서 int 배열의 중복을 제거하는법, hashSet을 int 배열로 변환하는 법을 알게되었다.


```java
package JAVAPRACTICE17;  
  
import java.util.ArrayList;  
import java.util.Arrays;  
import java.util.HashSet;  
import java.util.List;  
  
public class Solution {  
  
    List<List<Integer>> answer = new ArrayList<>();  
    List<Integer> sequence = new ArrayList<>();  
  
    public List<List<Integer>> threeSum(int[] nums) {  
          
        // 중복 제거  
        HashSet<Integer> set = new HashSet<>();  
        for (int x : nums) {  
            set.add(x);  
        }  
  
        search(Arrays.stream(set.toArray(new Integer[0]))  
                            .mapToInt(Integer::intValue)  
                            .toArray(),   
                        0);  
  
        return answer;  
    }  
  
    public void search (int[] nums, int level) {  
        if  (3 == sequence.size()) {  
            if (checkSum()) answer.add(new ArrayList<>(sequence));  
            return;  
        }  
  
        for (int i=level; i<nums.length; i++) {  
            sequence.add(nums[i]);  
            search(nums, i+1);  
            sequence.remove(sequence.size()-1);  
        }  
    }  
  
    public boolean  checkSum () {  
        int sum = 0;  
        for (Integer x : sequence) {  
            sum += x;  
        }  
        return 0 == sum;  
    }  
}
```


그런데 모든 수를 또 중복을 제거하면 안되었다. 같은 수의 중복이 0 이 되는 경우의 수를 포함해야 한다 
(nums = \[ -1, 0. 1. 2. -1. -4 ] 일때, -1, -1, 2 등)

결국 첫번째 방법이 맞았던 것 같다.

그런데 답지를 보니 내가 풀었던거와는 전혀 다른 3가지 방법이 있었다.
