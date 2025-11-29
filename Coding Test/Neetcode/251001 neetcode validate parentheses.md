```java
public class Solution {  
    public boolean isValid(String s) {  
       
        Stack<Character> st = new Stack<>();  
        char[] arr = {'(', ')', '[', ']', '{', '}'};  
  
        for (char ch : s.toCharArray()) {  
            if (st.isEmpty()) st.add(ch);  
            else {  
                if (ch == arr[1] || ch == arr[3] || ch == arr[5]) {  
                    int idx = 0;  
                    for (int i=0; i<arr.length; i++) {  
                        if (ch == arr[i]) {  
                            idx = i;   
                        }  
                    }  
                    if (st.peek() != arr[idx-1]) return false;  
                    st.pop();  
                } else {  
                    st.add(ch);  
                }  
            }  
        }  
  
        if (!st.isEmpty()) return false;  
  
        return true;  
    }  
}
```

