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
This code is __out of time__.  
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

### Increasing Triplet Subsequence
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/571/week-3-december-15th-december-21st/3570/)  
* candidate strategies:
```python
class Solution(object):
    def increasingTriplet(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        LEN_LIST = len(nums)
        
        for i in range(1,LEN_LIST-1):
            left, right = False, False
            middle_num = nums[i]
            for num in nums[:i]:
                if num < middle_num:
                    left = True
                    break
            for num in nums[i+1:]:
                if num > middle_num:
                    right = True
                    break
            if left * right == True:
                return True
        return False
```
This was accepted but took a lot of time because it contains a repeated search.  
The improved solution is here:

```python
class Solution(object):
    def increasingTriplet(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        
        min1 = min2 = 2**32
        for num in nums:
            min1 = min(min1, num)
            if min1 < num < min2:
                min2 = num
            if num > min2:
                return True
            
        return False
```

### Decoded String at Index
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/571/week-3-december-15th-december-21st/3572/)  
* candidate strategies:
```python
class Solution:
    def decodeAtIndex(self, S: str, K: int) -> str:
        
        prev_len = 0
        prev_str = ''
        
        for s in S:
            if s.isdigit():
                if prev_len * int(s) >= K:
                    idx = K % prev_len
                    return prev_str[idx-1]
                prev_len = prev_len * int(s)
                prev_str = prev_str * int(s)
            else:
                prev_len += 1
                if prev_len == K:
                    return s
                prev_str += s
```
This got time Time Limit Exceeded.  
The process of continuing to combine and increase the strings seems to have taken a lot of time.  
To eliminate this process, follow the steps below:
1. Count the length of the entire string.
2. Reverse the subset corresponding to Kth index.
<!---------
문자열을 계속 합치고 늘리는 과정에서 시간이 많이 소요된 것 같다.  
이러한 과정을 없애기 위해 아래와 같이  
1. 전체 문자열의 길이를 세고
2. K번째에 해당하는 하위 집합을 역추적 한다.
----->
```python
class Solution:
    def decodeAtIndex(self, S: str, K: int) -> str:
        
        decoded_str_len = 0
        
        # size of the total str
        for c in S:
            if c.isdigit():
                decoded_str_len *= int(c)
            else:
                decoded_str_len += 1
        
        for c in reversed(S):
            # break condition
            K %= decoded_str_len
            
            # update
            if c.isdigit():
                decoded_str_len /= int(c)
            else:
                if K == 0:
                    return c
                decoded_str_len -= 1
```
### Check Array Formation Through Concatenation
* problem: [link](https://leetcode.com/explore/challenge/card/january-leetcoding-challenge-2021/579/week-1-january-1st-january-7th/3589/)  
* candidate strategies:
```python
class Solution:
    def canFormArray(self,arr: List[int], pieces:):
        
        start_element = dict()
        for piece in pieces:
            start_element[piece[0]] = piece
        
        form_arr = []
        
        for num in arr:
            form_arr += start_element.get(num, [])
            print(form_arr)
        return form_arr == arr
```
<!-------
첫번째 원소만 저장하는 딕셔너리를 만들면 문제를 쉽게 해결할 수 있다.
---------->
Creating a dictionary that stores only the first element can solve the problem easily.