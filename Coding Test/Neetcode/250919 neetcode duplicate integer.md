```java
public boolean hasDuplicate(int[] nums) {
	HashSet<Integer> set = new HashSet<>();
	for (int it : nums) {
		if(set.contains(it)) return true;
		set.add(it);
	}
	return false;
}
```
