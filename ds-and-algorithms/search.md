search
==================

## tricky problems
### 4Sum II
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/571/week-3-december-15th-december-21st/3569/)  
* candidate strategies:
```python
from collections import defaultdict

class Solution(object):
    def fourSumCount(self, A, B, C, D):
        LEN_LIST = len(A)
        self.answer = 0
        sum_1 = defaultdict(int) # for A, B
        sum_2 = defaultdict(int) # for C, D
        
        for i in range(LEN_LIST):
            for j in range(LEN_LIST):
                sum_1[A[i] + B[j]] += 1
                sum_2[C[i] + D[j]] += 1
        
        # sort in ascending order
        answer = 0
        for key_1, val_1 in sum_1.items():
             for key_2, val_2 in sum_2.items():
                if key_1 + key_2 == 0:
                    answer += val_1 * val_2
                    break
        
        return answer
```
This code is out of time.  
From the __sort in ascending order__ part, It includes unnecessary search operations. 
For example, if the sum of two of the four was 4, 
all you had to do was check if there was a -4 and all the values were checked. 
This is an unnecessary. After changing it, the below was passed.
```python
# sort in ascending order
from collections import defaultdict

class Solution(object):
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        LEN_LIST = len(A)
        self.answer = 0
        sum_1 = defaultdict(int) # for A, B
        sum_2 = defaultdict(int) # for C, D
        
        for i in range(LEN_LIST):
            for j in range(LEN_LIST):
                sum_1[A[i] + B[j]] += 1
                sum_2[C[i] + D[j]] += 1
        
        # sort in ascending order
        answer = 0
        for key_1, val_1 in sum_1.items():
             if -1*(key_1) in sum_2:
                answer += sum_2[-1*key_1] * val_1

        return answer
```