```java
public class Solution {

    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> groups = new ArrayList<>();

        for (String str : strs) {
            if (groups.isEmpty()) {
                groups.add(new ArrayList<String>());
                groups.get(0).add(str);
            } else {
                boolean hasFound = false;
                for (List<String> group : groups) {
                    if (isAnagram(group.get(0), str)) {
                        hasFound = true;
                        group.add(str);
                    }
                }
                if (!hasFound) {
                    List<String> newGroup = new ArrayList<String>();
                    newGroup.add(str);
                    groups.add(newGroup);
                }
            }
        }
        return groups;
    }

    public boolean isAnagram (String s, String t) {
        char[] ss = s.toCharArray();
        char[] tt = t.toCharArray();

        Arrays.sort(ss);
        Arrays.sort(tt);

        return Arrays.equals(ss, tt);
    }
}

```

간단한 ArrayList 를 이용한 문제였던 것 같다. 난이도는 medium.

