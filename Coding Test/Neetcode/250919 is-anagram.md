```java
public static boolean isAnagram(String s, String t) {
	char[] ss = s.toCharArray();
	char[] tt = t.toCharArray();
	Arrays.sort(ss);
	Arrays.sort(tt);

	int i=0;
	while (i < (ss.length > tt.length ? ss.length : tt.length)) {
		if (i >= ss.length || i >= tt.length) return false;
		if (ss[i] != tt[i]) return false;
		i++;
	}
	return true;
}
```

```java
public class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        char[] sSort = s.toCharArray();
        char[] tSort = t.toCharArray();
        Arrays.sort(sSort);
        Arrays.sort(tSort);
        return Arrays.equals(sSort, tSort);
    }
}
```

Arrays.equals 가 있구나!

