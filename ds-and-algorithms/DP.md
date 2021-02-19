Dynamic Programming
==================
### Prerequisites
* optimal substructure: if an optimal solution can be constructed from optimal solutions of its subproblems
* overlapping subproblem

### General procedure
> __1. 최적해의 구조의 특징을 찾는다.__  
>   최적 부분구조를 찾은 후 부분문제에 대한 최적해를 그 문제에 대한 최적해를 구성하는 데 사용해야 함  
> __2. 최적해의 값을 재귀적으로 정의한다.__  
> __3. 최적해의 값을 계산한다.__   
>   테이블의 어떤 엔트리가 m[i,j]를 계산하는 데 사용되는지 결정하고 최적의 m[i,j]를 구함  
> __4. 계산된 정보들로부터 최적해를 구성한다.__  
>   필요에 따라 보조 테이블이 여러 개 필요할 수 있음

### Kadane's Algorithm
* arr: list of int
* Let K be solution of arr[:i]
* K[i] = max(K[i-1] + arr[i], arr[i])
* See also: [here](https://hwan-shell.tistory.com/m/117?category=771708)

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

## tricky problems
### Cherry Pickup II
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/571/week-3-december-15th-december-21st/3571/)  
* candidate strategies:

### Burst Balloons
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/570/week-2-december-8th-december-14th/3564/)  
* candidate strategies:

### Palindrome Partitioning
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/570/week-2-december-8th-december-14th/3565/)  
* candidate strategies:

### Arithmetic Slices
* problem: [link](https://leetcode.com/explore/challenge/card/february-leetcoding-challenge-2021/586/week-3-february-15th-february-21st/3644/)
* solution:
* 처음에 i, j를 for loop를 돌며 체크했으나 시간 초과
```python
class Solution:
    def numberOfArithmeticSlices(self, A):
        
        len_list = len(A)
        dp = [0 for _ in range(len_list)]
        for i in range(2, len_list):
            if A[i] - A[i-1] == A[i-1] - A[i-2]:
                dp[i] = dp[i-1] + 1
        
        return sum(dp)
```
* element 하나를 추가하였을 때 i+1보다 arithmetic series가 n+1개 더 많아짐
* 여기서 n은 i번째의 arithmetic series 수