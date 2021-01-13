Linked list
==================
### Singly linked list
<img src='https://user-images.githubusercontent.com/48520885/103737302-47060e00-5035-11eb-825e-b4726650cc81.png'>
  
#### Inserting at the head
> 1. Allocate a new node
> 2. Insert new element
> 3. Have new node point to old node
> 4. Update head to point to new node
#### Removing at the head
> 1. Update head to point to next node in the list
> 2. Allow garbage collector to reclaim the former first node
#### Inserting at the Tail
> 1. Allocate a new node
> 2. Insert new element
> 3. Have new node point to null
> 4. Have old last node point to new node
> 5. Update tail to point to new node
#### Removing at the Tail
> * Removing at the tail of a singly linked list is not efficient!
> * There is no constant time way to update the tail to point to the previous node
 
## tricky problems
### Merge Two Sorted Lists
어려운 문제는 아닌 것 같은데 처음 풀어봐서 시간이 오래 걸렸다. 주의할 점은
> 1. merge시 하나에 다른 하나의 list를 append하는 것보다 새로운 listnode를 선언하는 게 메모리는 많이 들어도 직관적이다.
> 2. next로 다른 listnode를 가리킬 때 해당 listnode의 next가 필요한 것인지 확인하고 필요 없다면 null을 가리키도록 바꿔야 한다.
* problem: [link](https://leetcode.com/explore/challenge/card/january-leetcoding-challenge-2021/579/week-1-january-1st-january-7th/3592/)  
* solution:
```python
# Definition for singly-linked list.
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        answer = ListNode(val=-2**32)
        answer_p = answer
        
        while l1 or l2:
            # 하나라도 nonetype이면
            if not l1:
                answer.next = l2
                l2 = l2.next
                answer = answer.next
            elif not l2:
                answer.next = l1
                l1 = l1.next
                answer = answer.next
            elif l1.val < l2.val:
                answer.next = l1
                l1 = l1.next
                answer = answer.next
            else:
                answer.next = l2
                l2 = l2.next
                answer = answer.next
        
        return answer_p.next
```