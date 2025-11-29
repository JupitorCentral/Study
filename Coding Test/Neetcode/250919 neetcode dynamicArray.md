```java
class DynamicArray {
	Integer[] arr;
	int cap;

	public DynamicArray(int capacity) {
		this.cap = capacity;
		arr = new Integer[this.cap];
	}

	public int get(int i) {
		return arr[i].intValue();
	}

	public void set(int i, int n) {
		arr[i] = Integer.valueOf(n);
	}

	public void pushback(int n) {
		int i = cap;
		while(i > 0 && arr[i-1] == null) i--;
		if (i == cap) {
			this.resize();
		}
		this.set(i, n);
	}

	public int popback() {
		int i = cap;
		while(i > 0 && arr[i-1] == null) i--;
		int x = get(i-1);
		arr[i-1] = null;
		return x;
	}

	private void resize() {
		cap *= 2;
		arr = Arrays.copyOf(arr, cap);
	}

	public int getSize() {
		int size = 0;
		for (Integer it : arr) {
			if (it != null) size++;
		}
		return size;
	}

	public int getCapacity() {
		return cap;
	}
}
```

문제를 좀 더 꼼꼼히 확인했었어야 했다.
답을 보니 내가 너무 복잡하게 생각했고... 아니 근데 너무 예외를 신경안쓰는 DynamicArray 잖아. 실용적이지 않은.
그리고 resize 해야 한다는게 내가 자꾸 1개만 늘려버리니까, 문제가 됬었다.
구현한 resize 함수를 사용하라는 얘기였다. 젠장...


아니 근데 solution 이 너무 답이 없다 이거. 잘못된 코드다.

```java
class DynamicArray {

    private int[] arr;
    private int length;
    private int capacity;

    // Constructor to initialize the dynamic array
    public DynamicArray(int capacity) {
        this.capacity = capacity;
        this.length = 0;
        this.arr = new int[this.capacity];
    }

    // Get value at the i-th index
    public int get(int i) {
        return arr[i];
    }

    // Insert value n at the i-th index
    public void set(int i, int n) {
        arr[i] = n;
    }

    // Insert n in the last position of the array
    public void pushback(int n) {
        if (length == capacity) {
            resize();
        }
        arr[length] = n;
        length++;
    }

    // Remove the last element in the array
    public int popback() {
        if (length > 0) {
            // soft delete the last element
            length--;
        }
        return arr[length];
    }

    // Resize the array
    private void resize() {
        capacity *= 2;
        int[] newArr = new int[capacity];
        for (int i = 0; i < length; i++) {
            newArr[i] = arr[i];
        }
        arr = newArr;
    }

    // Get the size of the array
    public int getSize() {
        return length;
    }

    // Get the capacity of the array
    public int getCapacity() {
        return capacity;
    }
}

```

이렇게 되면 
capacity = 3 라고 했을때
set (1, 1) 을 하면 length 가 2이어야 되는데 실제적으로 는 0이다.
(null 1 null)
pushback 을 할때만 length 가 증가하니깐.
그럼 말이 안되잖는가. 
그럼 length 가 의미하는바가 뭔데 ??
여기서 push back 을 한다 ?

null 1 null 해서 length 가 여전히 1이 된다.

바보같은 답이다 이건.