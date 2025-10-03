
순서를 안 신경써도 되기 때문에
Map 을 써서 풀 순 있을 것이다.

left 가 0 부터 s2.length() - s1.length() 까지 순회화면서
left ~ left + s1.length() 의 캐릭터를 순회하면서 Map 에 저장해서
s1 을 순회한 Map 과 비교하면 판별할 수 있을 것이다.

그런데 그렇게 되면 
m 은 중복이 없는 s1 의 글자들이라 했을때
space complexity 가 $O(m)$ 이 되고
time complexity 는 $O(n\cdot \:m)$ 이 된다.

다른 방법이 없을까 ..?
이것도 sliding window 로 되나 ..? 애초에 위에 Map 으로 푸는 것도 sliding window 인데 ?

아 근데 내가 생각했던 boolean 은 아니지만 (s1 의 캐릭터가 중복이 될 수 있나보다)
만약 사용하는 메모리의 크기가 상수인 경우 $O (1)$ 가 되니까 그렇게 했나보다.
처음 아이디어가 맞았다.

```java
public int alphabetCount = 'z' - 'A' + 1;  
  
public boolean checkInclusion(String s1, String s2) {  
    int[] a1 = new int[alphabetCount];  
    int[] a2 = new int[alphabetCount];  
  
    for (char ch : s1.toCharArray()) {  
        a1[ch-'A']++;  
    }  
    for (int i=0; i<Math.min(s1.length(), s2.length()); i++) {  
        System.out.println(s2.charAt(i));  
        a2[s2.charAt(i)-'A']++;  
    }  
  
    if (check(a1, a2)) return true;  
  
    int r = s1.length();  
    while (r < s2.length()) {  
        a2[s2.charAt(r-s1.length()) - 'A']--;  
        a2[s2.charAt(r) - 'A']++;  
        if (check(a1, a2)) return true;  
  
        r++;  
    }  
  
    return false;  
}  
  
public boolean check (int[] a1,  int[] a2) {  
    for (int i=0; i<alphabetCount; i++) {  
        if (a1[i] != a2[i]) return false;  
    }  
    return true;  
}
```


답지에서는 처음부터 s2 의 스트링 길이가 s1 보다 작으면 false 를 리턴해버리고 시작한다.
즉 s2.length() >= s1.length() 로 가정.
그리고 문제에서는 lowercase 만 나온다고 하므로 비교배열의 크기를 26으로 설정했다 
('z' - 'a' + 1, 즉 알파벳숫자가 26개인가보다)


```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) {
            return false;
        }

        int[] s1Count = new int[26];
        int[] s2Count = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1Count[s1.charAt(i) - 'a']++;
            s2Count[s2.charAt(i) - 'a']++;
        }

        int matches = 0;
        for (int i = 0; i < 26; i++) {
            if (s1Count[i] == s2Count[i]) {
                matches++;
            }
        }
        // 먼저 처음 s1 크기만큼 비교

        int l = 0;
        
        // 처음 for 문을 진입할때 r 의 값은 s1 길이만큼 비교하고 난 다음 오른쪽으로 이동한 위치.
        for (int r = s1.length(); r < s2.length(); r++) {
            if (matches == 26) {
                return true;
            }

			// 내 코드와는 다른 match 방식. 더 최적화 되어있음.	
            int index = s2.charAt(r) - 'a';
            s2Count[index]++;
            if (s1Count[index] == s2Count[index]) {
                matches++;
            } else if (s1Count[index] + 1 == s2Count[index]) {           // 이게 좀 핵심인 것 같다. else 를 안쓰기 때문.
                matches--;                                      // s1Count 에서 해당 캐릭터에 대해 1 더한게 s2Count 와 같다는 말은
            }                                                                  // s1Count 가 해당 캐릭터에 대해서 한개 덜 카운트 되어 있다 -> 안맞다는 얘기.
							            // 근데 왜 else 를 안쓸까?

            index = s2.charAt(l) - 'a';
            s2Count[index]--;
            if (s1Count[index] == s2Count[index]) {
                matches++;
            } else if (s1Count[index] - 1 == s2Count[index]) {
                matches--;
            }
            l++;
        }
        return matches == 26;
    }
}
```


근데 내 풀이가 보기는 더 좋아보여도,
이 코드가 더 check 를 하는데에 있어 더 최적화 되어 있다.

내 코드에서 위와 같은 방식으로 더 최적화 할 수 있을 것 같다.

근데 

```java
if (s1Count[index] == s2Count[index]) {
	matches++;
} else if (s1Count[index] + 1 == s2Count[index]) {           // 이게 좀 핵심인 것 같다. else 를 안쓰기 때문.
	matches--;                                      // s1Count 에서 해당 캐릭터에 대해 1 더한게 s2Count 와 같다는 말은
}              
```

여기서 왜 else 를 안쓰는걸까?

고민을 좀 더 해봐야 겠다.

