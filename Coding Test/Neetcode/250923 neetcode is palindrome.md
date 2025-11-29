```java
public boolean isPalindrome(String s) {  
  
    StringBuilder sb = new StringBuilder();  
  
    for (char ch : s.toCharArray()) {  
        if (ch >= 'A' && ch <= 'z' || ch >= '0' && ch <= '9') {  
            sb.append(ch);  
        }  
    }  
  
    char[] target  = sb.toString().toLowerCase().toCharArray();  
    int i = 0;  
    int j = target.length-1;  
  
    while (i < j) {  
        println("i : " + i + ", " +  target[i] +   ",  j : " + j + ", " + target[j]);  
        if (target[i] == target[j]) {  
            i++;  
            j--;  
        } else {  
            return false;  
        }  
    }  
  
    return true;  
}
```

two pointer 에서 조건이 약간 추가된 palindrome 을 구하는 문제 

