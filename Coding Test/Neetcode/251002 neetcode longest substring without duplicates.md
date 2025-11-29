

맨처음에는 HashSet 을 썼었다가
```java
public int lengthOfLongestSubstring(String s) {  
  
    Set<Character> set = new HashSet<>();  
    int maxLength = 0;  
  
    for (char ch : s.toCharArray()) {  
        if (set.contains(ch)) {  
            set = new HashSet<>();  
            set.add(ch);  
        } else {  
            set.add(ch);  
            maxLength = Math.max(set.size(), maxLength);  
        }  
    }  
  
    return maxLength;  
}
```

이내 이런 케이스가 있다는 것을 알게 되었다.

-> a b c d b a e g h

위에 코드대로라면  a b c d 에서 b 가 들어오는순간 set 이 새로 선언되어 set 에 b 만 들어간다.
하지만 답은 c d b a e g h 이다.

따라서, 순서를 기억하고 처음부터 이미 존재했던 캐릭터까지 순회하면서 모두 remove 하고 나서,
새로 queue 에 저장하면 됬었다.

```java
public int lengthOfLongestSubstring(String s) {  
  
    Queue<Character> q = new LinkedList<>();  
    int maxLength = 0;  
  
    for (char ch : s.toCharArray()) {  
        if (q.contains(ch)) {  
            while (q.peek() != ch) {  
                q.remove();  
            }  
            q.remove();  
            q.add(ch);  
        } else {  
            q.add(ch);  
            maxLength = Math.max(q.size(), maxLength);  
        }  
    }  
  
    return maxLength;  
}
```

순회하면서 계속 queue 에 contains 를 호출하다보니
time complexity 가 O (n x m)  이 된다. 
space complexity 는 O(m)

즉 contains 가 O( 1 ) 이어야 한다는 얘기인데.
즉 Set 의 contains complexity 가 O ( 1 ) 이므로 queue 를 사용해야 한다는 결론이 나온다.

다시 Set 으로 돌아가야 한다.
대신, 순서를 유지해야 한다.
그렇다면 순서는 포인터를 사용하면 된다.


```java
public int lengthOfLongestSubstring(String s) {  
  
    Set<Character> charSet = new HashSet<>();  
    int maxLength = 0;  
  
    int left = 0;  
  
    char[] arr = s.toCharArray();  
  
    for (int right = 0; right<arr.length; right++) {  
        char ch = arr[right];  
  
        if (charSet.contains(ch)) {     // O ( 1 )  
            while (arr[left] != ch) {  
                charSet.remove(arr[left++]);  
            }  
            left++;  
        } else {  
            charSet.add(ch);  
            maxLength = Math.max(charSet.size(), maxLength);  
        }  
    }  
  
    return maxLength;  
}
```

성공이다.