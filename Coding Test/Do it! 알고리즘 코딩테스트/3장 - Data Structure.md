
## 문제 4 

x is row and y is column

### My Solution

x1, y1 -> x2, y2

First, get partical sum of y1~y4 and then

from (x1, y1) -> to (x2, y2) 

add all partial sum from x2 to x1 
for each rows from y2 to y1

so actually I didn't get x-y partial sum from $(x_0, y_0)$  to $(x, y)$.
My $S(x,y)$ was partial sum from $S(x_0, y_0)$ to $S(x_0, y)$

in some ways it's just different, but looking closely it's wrong

![[Screenshot 2024-11-17 at 11.56.09 PM.png]]

$D(2,3)$ should represent sums of all elements from element $(0,0)$ to $(2,3)$

### Resolve problem again using accurate computation of 2D cumulative sums


![[Screenshot 2024-11-18 at 1.29.21 PM.png|400]]

#### cumulative sum 
$mat$ is matrix from input

$s(x,y)$ = $mat[x-1,y-1] + s[x-1][y] + s[x][y-1] - s[x-1][y-1]$ 

Calculate this for all x,y of s, starting from $(1,1)$ for each x,y, and each zero-element (x or y of element is zero) should be 0 and not be calculated. (out of index)

For all points $(x,y)$ in $S$, starting from $(1,1)$, calculate this for all $x, y$ , where zero-elements (cases where either x or y is 0) should be 0 and must no be calculated (out-of-index)

#### 2d cumulative sums between two points

$s = s[x_2][y_2] + s[x_2][y_1] + s[x_1-1][y_2] -s[x_1-1][y1-1]$


## 문제 6 Computing consecutive Integer sums 

### _Two pointer_


## 문제 7 Jumong's order

### _Sort -> Tow Pointer_

Arrays.sort -> $nlog(n)$

## 문제 8 Finding a good number

It's not to find all combinations, but how many numbers that satisfy the condition are there.
(Count the numbers that can be expressed as sums, instead of generating all cases.)

## 문제 9 DNA password

### _Sliding Window_

Moving window - using two points following separate moving patterns


![[Screenshot 2024-11-18 at 9.53.14 PM.png]]

-> Time complexity -  $O(n^2)$
-> This approach wasn't sliding window at all
-> Sliding window implementation should only modify boundary pointers -> $O(n)$
-> input - using chararray

![[Screenshot 2024-11-18 at 10.42.41 PM.png|500]]


![[Screenshot 2024-11-18 at 10.42.54 PM.png|600]]

내가 책보다 훨씬 잘한 것 같다.
I think I did much better than expected, than the book
I deserve to give myself a pat on the back

-----------

$1 <= |P| <= |S| <= 1,000,000$
Hence, |S| can be 1, 2, 3 

## 문제 10 _Finding minimum value_  ★★

deque (덱) -> It's really fascinating



Nodes are added from the right, 
and if an existing value is larger than the newly entered value, (when the incumbent value exceeds the incoming value)
it is discarded.
Then the leftmost node becomes the minimum value.
As the window slides, 
(As the window traverses, During window progression, as the window advances)
if the index of the leftmost index goes out of the window range, 
(exceeds the window boundaries, falls outside the window scope)
it is discarded.

window -> l, r -> two pointer

_BufferedWriter_ : for printing all answers at once 
(used to put output in a buffer and print it out all at once)
(Used to buffer output for consolidated printing)

_Node_ : index , value

1. To optimize performance, data is read directly from input buffer rather than storing every elements in the array (using nextToken)
   
2. Discard node from deque of which index is out of boundaries
   0, 1, 2, 3 if L is 3, if n is 3 this time, node of which index is 0 should be discarded 
   we don't have to save left boundary of window, just calculate this using current position
   
3. Define new Node
   
4. Starting from the end, remove all nodes whose value is greater than new one.
   
5. insert insert current node
   
6. Write down value of first node of Deque to BufferedWriter

![[Screenshot 2024-11-19 at 6.18.33 PM.png|550]]


## 문제 11 Creating an ascending sequence using a stack

Took a long time to understand the problem
used StringBuffer

![[Screenshot 2024-11-20 at 2.27.04 PM.png|400]]


## 문제 12 Computing NGE (next greater element)

using stack
1. first number -> save value to a number array, and push index to stack
2. for next number : if upcoming number is greater than value of that index, then pop it 
   -> and save upcoming value to answer array in the position of index. repeat this until stack is not empty and $arr[stack.peek()] < new\_number$
3. push index to stack, and save new number to number array (arr)
4. repeat this n times
5. after that repeat, for every remains in stacks, (which holds indexes), 
   pop it and save -1 to answer array in the position of popped index
6. print answer array

스택을 사용하기
arr - 숫자 배열
ans - 정답 배열
1. 첫번째 수 -> stack 이 비어있으므로 push
2. 두번째 ~ n번째 수 -> 만약 새로 들어온 값이 stack의 가장 위에 있는 인덱스에 해당하는 숫자배열의 값보다 클 경우, 스택을 팝하고, 해당 정답배열의 해당 인덱스 위치에 새로 들어온 값을 저장한다. 
   이걸 스택이 비어있지 않을때까지 반복한다.

Using stack
arr - number array
ans - answer array

1. For each subsequent number -> if the new incoming value is greater than the value in the number array corresponding to the index at the top of the stack, pop the stack and store the new incoming value in the answer array at the position of that popped index. Repeat this process while stack is not empty
   (when first, this won't happen as stack should be empty)
2. push index to stack and save number to arr
3. repeat above for all subsequent numbers
4. After that, if stack is not empty, so repeat this : 
   pop stack and put -1 to answer array at the position of popped index
5. print answer array

- arr holds the original sequence of numbers
- ans will hold the NGE for each position
- For the first number, we simply push it to the empty stack
- For each subsequent number, we check if it's greater than the number corresponding to the index at the stack's top
- If it is greater, we've found an NGE, so we:
    - Pop the index from the stack
    - Store the new number as the NGE in our answer array at that index
    - Keep doing this until either the stack is empty or we find a number that's bigger than our current one



![[Screenshot 2024-11-21 at 10.29.06 PM.png|500]]




## 문제 13 Card Game


There are N cards, numbered consecutively from 1 to N, with card 1 on top and card N at the bottom. The following actions are repeated until only one card remains:

First, throw away the top card. Then, move the new top card to the bottom of the deck. For example, when N = 4, the cards are initially arranged as 1, 2, 3, 4 from top to bottom.

After discarding 1, cards 2, 3, 4 remain. Moving 2 to the bottom results in 3, 4, 2. After discarding 3, 4 and 2 remain. Moving 4 to the bottom gives 2, 4. Finally, after discarding 2, only card 4 remains.

Write a program that determines which card remains last when given N.



## 문제 14  Implementing Absolute Value Heap

An absolute value heap is a data structure that supports the following operations:

1. Insert an integer x (x ≠ 0) into the array
2. Output and remove the value with the smallest absolute value from the array. If there are multiple values with the same smallest absolute value, output and remove the smallest actual number among them.

Write a program to implement an absolute value heap, starting with an empty array.

_Comparator of PriorityQueue_

$o_1, o_2$ -> { 
	if return negative : $o_1$ comes first (Show first when polling from queue)
	equal : same priority
	if return positive : $o_2$ comes first
}




