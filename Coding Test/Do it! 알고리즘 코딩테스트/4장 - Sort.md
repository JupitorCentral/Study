
## Bubble sort

i -> 0 ~ (n-1)
j -> 0 ~ (n - (i+1))
ex) n = 5, i : 0 1 2 3 4
j : when i = 0, 0 1 2 3

compare $a[j]$ and $a[j+1]$

after loop of index i,
$a[n-i]$ should be maximum number among remained numbers.


## 문제 16 Bubble Sort program 1

Can't understand the idea of solving this problem at all


A boy wrote the following bubble sort program in C++.

![[Screenshot 2024-11-22 at 4.21.35 PM.png|300]]


In the above code, n is the size of the array, and a is an array containing numbers. Numbers are filled starting from index 1 of the array.
Write a program that determines what value will be output when executing the above code.

(We need to rewrite this program because it will result in timeout, as time complexity of Bubble sort is $O(n^2)$. )

This is a problem about determining when a loop occurs in bubble sort where no swaps take place. As mentioned in the core theory, when no swaps occur during an entire inner for-loop of the bubble sort's double for-loop, it indicates that all data is already sorted. At this point, we can immediately terminate the process to reduce time complexity. However, since the maximum range of N in this problem is 500,000, solving it with bubble sort might exceed the time limit. We need a different approach to calculate how many times the inner for-loop was executed.

The inner loop performs swaps while moving from left to right, from 1 to n-j. This means that the maximum distance a specific data point can move left during the inner loop is 1. Therefore, we can solve this problem by comparing the pre-sort and post-sort indices of each data point to find the value that moved leftward the most.

## Selection Sort

1. find minimum or maximum value
2. swap that number with position of current index, and index will move from 0 to n-2 
   (as n-1 is last number and don't have to be sorted)

## 문제 17 Sorting digits in descending order

![[Screenshot 2024-11-22 at 10.17.50 PM.png|300]]


## Insertion Sort

after nth times iterations, n number is sorted from the left.
start with 1st, (array is zero-indexed)
assume zero index element is sorted, find right position of selected element, and put it.
when moving the selected element to the correct position, the elements to the right of the correct position are pushed back in order.
_( back in order : "Back" here means "toward the rear position," and "in order" means "sequentially/one by one." So "back in order" means elements are moved toward the right direction one by one, maintaining their relative sequence. )_
(Placing an element in its correct position causes a cascading shift of all following elements)


## 문제 18 Calculating ATM withdrawal time

we gonna solve this problem greedily. when person who took most short time comes first, and line up following people in ascending order by times taken, we can get fastest time total.

so I'm gonna sort people using selection sort, and calculate cumulative sums, and add all elements.

from 1 to n-1 (last element)
	find correct position using j, j starts from 0 to i-1
	(using while $a[i] < a[j]$, for ascending order)
	shift all elements to right of correct position, using another index k.
	save $a[i]$ 
	k start from i-1 and end at j, and $a[k+1] = a[k]$ 
	put $a[i]$ to $a[j]$


## Quick Sort  ★★

average time complexity is $O(nlog(n))$


## 문제 19 Find kth number  ★★

partitioning is the key

![[Screenshot 2024-11-24 at 10.03.12 PM.png|300]]

![[Screenshot 2024-11-24 at 10.03.41 PM.png|400]]



## Merge Sort

![[Screenshot 2024-11-24 at 12.46.14 PM.png|450]]


## 문제 20 수 정렬하기 2

그냥 merge sort 갈기는 것


## 문제 21 버블 소트 프로그램 2  ★★

![[Screenshot 2024-11-25 at 8.52.27 PM.png|400]]

swaps += j - i 가 아니라
swaps += j - 1 - j 인 이유 : swaps 가 계산될때 이미 j 는 j++ 로 인해 plus 1 인 상황이기 때문


## 기수 정렬

누적합 : 각 버켓의 마지막 수가 있어야 할 위치 + 1 을 나타냄
 0 - 1
 1 - 3
2 - 2
이렇게 있다 치면 누적합은

s0 = 1
s1 = 4
s2 = 6

이러면, 
s0 s1 s1 s1 s2 s2 
 0   1   2  3  4   5

그리고 우린 앞에 있는 수가 정렬된 배열에서 더 앞에 있게 하기 위해서
초기 배열의 뒤에서부터 나열함
이러면 배열 앞쪽에 있는 수가 정렬된 배열에서도 같은 버켓에 있는 수들 중에 가장 앞에 위치하게 됨

max digit 구하기

- digit count < max digit
- computing bucket
- change elements of bucket to cumulative sums 

- for every elements from last to start in number array,

	calculate sorted array, using each index derived from element of bucket at the position of proper current element ( $pos = (arr[i] / digit ) \% 10$ )

- bucket\[pos]--

## 문제 22 수 정렬하기 3

Array 와 정수 index 의 동작으로 큐의 동작을 구현 -> 이게 골때린다

bucket : 각 자릿수의 빈도
bucket 을 합배열로 만듦 $bucket[i]$ += $bucker[i-1]$ 

$output[bucker[(arr[i] / digit \% 10)] - 1]$ = $A[i]$
$bucket[(arr[i] / digit) \% 10]$ -- ;

씨발 이걸 어떻게 생각해 ?

-> 다음날 이해 완료






