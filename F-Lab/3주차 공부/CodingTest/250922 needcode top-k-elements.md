
```java
public int[] topKFrequent(int[] nums, int k) {  
  
    int[] answer = new int[k];  
  
    Map<Integer, Integer> map = new HashMap<>();  
    for (int i=0; i<nums.length; i++) {  
        int x = nums[i];  
        map.merge(x, 1, Integer::sum);  
    }  
  
    List<Integer> sorted = map.entrySet().stream().sorted(Map.Entry.<Integer, Integer>comparingByValue().reversed())  
    .map(Map.Entry::getKey)  
    .collect(Collectors.toList());  
  
    for (int i=0; i<k; i++) {  
        answer[i] = sorted.get(i);  
    }  
    return answer;  
}
```


무슨 자료구조를 사용해야할지 도저히 모르겠어서 
AI 를 이용해서 풀었다. 근데 O( n ) 시간복잡도를 활용해서 풀라는데...

아씨 답도 Time complexity 가 `n log n` 이다. 뭐야...





```java
public class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }

        List<int[]> arr = new ArrayList<>();
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            arr.add(new int[] {entry.getValue(), entry.getKey()});
        }
        arr.sort((a, b) -> b[0] - a[0]);

        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = arr.get(i)[1];
        }
        return res;
    }
}
```


