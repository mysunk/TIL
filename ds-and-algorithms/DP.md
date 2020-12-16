Dynamic Programming
==================

### Kadane's Algorithm
* arr: list of int
* Let K be solution of arr[:i]
* K[i] = max(K[i-1] + arr[i], arr[i])
* See also: [here](https://hwan-shell.tistory.com/m/117?category=771708)

### Prerequisites
* optimal substructure: if an optimal solution can be constructed from optimal solutions of its subproblems
* overlapping subproblem

### Two approaches [[ref](https://velog.io/@hanturtle/%EB%8F%99%EC%A0%81%EA%B3%84%ED%9A%8D%EB%B2%95)]
#### 1) Top-down
* recursion
* using memoization
* function call overhead
* stack (function call) + heap (table)
* generally, available heap memory is much larger than stack memory
  * heap: 256 Mb
  * stack: 1 Mb

#### 2) Bottom-up
* approach to filling up __DP table__
* have to filling up all the elements of table
* heap (table)

#### Can every problem on Dynamic Programming be solved by both top-down and bottom-up approaches?  
* answer: yes
* from [here](https://www.quora.com/Can-every-problem-on-Dynamic-Programming-be-solved-by-both-top-down-and-bottom-up-approaches)
