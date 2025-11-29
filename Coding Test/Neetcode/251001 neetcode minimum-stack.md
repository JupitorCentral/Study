```java
class MinStack {  
  
    ArrayList<Integer> stack;  
    ArrayList<Integer> minStack;  
  
    public MinStack() {  
        stack = new ArrayList<>();   
        minStack = new ArrayList<>();  
    }  
      
    public void push(int val) {  
        if (minStack.isEmpty()) minStack.add(val);  
        else {  
            if (val < this.getMin()) {  
                minStack.add(val);  
            }  
        }  
        stack.add(val);  
    }  
      
    public void pop() {  
        int lastValue = stack.get(stack.size()-1);  
        if (lastValue == this.getMin()) minStack.remove(minStack.size()-1);  
        stack.remove(stack.size()-1);  
    }  
      
    public int top() {  
        return stack.get(stack.size()-1);  
    }  
      
    public int getMin() {  
        return minStack.get(minStack.size()-1);  
    }  
}
```

이럴땐 아이패드에다가 상황을 그려보면서 해보는게 좋을 것 같다.
머릿속으로 시뮬레이션하면 제일 좋겠지만.

아무튼 이런 해결방법도 있구나 하고 새로 알아가는게 좋을 것 같다.

답지에서는 아예 ArrayList 가 아니라 stack 2개를 내부에 써버린다.

```java
public class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int val) {
        stack.push(val);
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }

    public void pop() {
        if (stack.isEmpty()) return;
        int top = stack.pop();
        if (top == minStack.peek()) {
            minStack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```


One Stack 으로 하는 방법도 있는데, 이건 상당히 놀랍다.

```java
public class MinStack {
    long min;
    Stack<Long> stack;

    public MinStack() {
        stack = new Stack<>();
    }

    public void push(int val) {
        if (stack.isEmpty()) {
            stack.push(0L);
            min = val;
        } else {
            stack.push(val - min);
            if (val < min) min = val;
        }
    }

    public void pop() {
        if (stack.isEmpty()) return;

        long pop = stack.pop();

        if (pop < 0) min = min - pop;
    }

    public int top() {
        long top = stack.peek();
        if (top > 0) {
            return (int) (top + min);
        } else {
            return (int) min;
        }
    }

    public int getMin() {
        return (int) min;
    }
}
```

```java
long min;
Stack<Long> stack;
```

##### push

```java
public void push(int val) {
	if (stack.isEmpty()) {
		stack.push(0L);
		min = val;
	} else {
		stack.push(val - min);
		if (val < min) min = val;
	}
}
```

push 할때, 들어가는 값은 들어가려는 값을 v 라고 하고, 현재 최솟값을 min 이라 할때
push (v - min) 이다.
이떄, v < min 이면 v - min < 0 이 이므로 음수가 들어가고,
v > min 이면 v - min > 0 이므로 양수가 된다. 

그래서 나중에 pop 을 할때,
stack.peek() < 0 이면
-> push 된 값이 이전 최솟값보다 더 적어서  최솟값이 변했다는 의미
그래서 이전 최솟값을 pMin 이라 할때,

stack.peek() = min - pMin.
따라서 이전 최솟값 = pMin = min - stack.peek()
(min 업데이트)

그리고 popped value 는 stack.peek() + pMin (=min, 이전 최솟값으로 업데이트 된 min)

stack.peek() > 0 이면 
stack.peek() = value when pushed - min
따라서 popped value = stack.peek() + min

따라서 pop()일떄
stack.peek() > 0 이든 stack.peek() < 0 이던
stack.peek() + min 동작 수행.

stack.peek() 일때는 min 을 업데이트 하나 안하나 차이가 없음.


##### pop
```java
public void pop() {
	if (stack.isEmpty()) return;

	long pop = stack.pop();

	if (pop < 0) min = min - pop;
}
```

##### top

```java
public int top() {
	long top = stack.peek();
	if (top > 0) {
		return (int) (top + min);
	} else {
		return (int) min;
	}
}
```

top -> 음수면, 최솟값이 업데이트 된 것. 즉, min 이 가장 위에 올라온 값이다.
양수면, min 이 업데이트 되지 않았으므로, pushed value 는 value - min 이다.
따라서 popped value 는 value + min.

등호는 이전 최솟값과 새로 들어온 최솟값이 같다는 의미 -> 그대로 min 리턴.



![[251001 neetcode minimum-stack freeform.pdf]]



하나의 stack 방법에서 int 에 대한 overflow 가 발생.
long 으로 바꾸어야 한다.

어떤 부분에서 overflow 가 발생하는걸까?


```java
public class MinStack {  
  
    Stack<Long> st;  
    long min;  
  
    public MinStack() {  
        st = new Stack<>();  
    }  
      
    public void push(int val) {  
        if (st.isEmpty()) {  
            st.add(0L);        // you shouldn't add val itself for the top - which is 'peak + min'  
            min = val;  
        } else {  
            st.add(val - (long)min);     // saving previous min value    
                                // min = -2147483648, val = 2147483647, if int, overflow.  
            if (min > val) min = val;  
        }  
    }  
      
    public void pop() {  
        long top = st.peek();  
        if (top < 0) {  // min is updated  
             // top = value (current min) - previous min           
             // return value = top + previous min            
             // previous min = current min - top            
             // return value = top + (current min - top) = current min;            min = min - top;        
             // min = 2147483647, top = min = -2147483648  -> if int, overflow  
        }  
        // As we don't have to return peek value, only update min.  
        st.pop();  
    }  
      
    public int top() {  
        long peek = st.peek();  
  
        if (peek > 0) {  // case where previous value doesn't change  
            return (int)(peek + min);  
        } else {  
            return (int)min;  
        }  
    }  
      
    public int getMin() {  
        return (int)min;  
    }  
}
```


min = -2^31, val = 2^31 이면 2^32 로 int  를 넘어버린다.
(push -> val - min)
