Binary tree
==================

## Problems
### Smallest Subtree with all the Deepest Nodes
* problem: [link](https://leetcode.com/explore/challenge/card/december-leetcoding-challenge/570/week-2-december-8th-december-14th/3563/)  
* solution:
```python
class Solution:
    def subtreeWithAllDeepest(self, root: TreeNode) -> TreeNode:
        def dfs(node):
            # if leaf
            if not node:
                return node, 0
            # traverse
            left_subtree, left_height= dfs(node.left)
            right_subtree, right_height= dfs(node.right)
            # compare
            if left_height == right_height:
                return node, left_height+1
            if left_height > right_height:
                return left_subtree, left_height+1
            else:
                return right_subtree, right_height+1
        return dfs(root)[0]
```
* tips:
    * 한 노드를 기준으로 왼쪽 subtree의 height와 오른쪽 subtree의 height가 같을 때 해당 노드가 기준을 충족
    * height가 다를 때는 height가 같았던 node를 계속해서 불러옴