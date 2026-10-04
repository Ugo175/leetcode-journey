Problem: Same Tree

Difficulty: Easy

### U —> Understand
We need to determine if two binary trees are identical:
- Input: roots of two binary trees (p and q)
- Output: True if the trees are identical, False otherwise
- Trees are identical if they have the same structure and same node values
- Empty trees are considered identical
- Edge cases: both empty, one empty one not, different structures, same structure different values

### M —> Match
This is a recursive tree comparison problem:
- We can use DFS to traverse both trees concurrently
- At each step, we compare the current nodes
- We recursively check both left and right subtrees
- Pattern: Concurrent recursive traversal for tree comparison

### P - Plan
1. Define a recursive helper function that takes both tree nodes
2. Base cases:
   - If both nodes are None, return True (both empty)
   - If only one node is None, return False (structure mismatch)
   - If values don't match, return False (value mismatch)
3. Recursive case:
   - Check if left subtrees are identical
   - Check if right subtrees are identical
   - Return True only if both subtrees are identical
4. Call the helper function on both roots

### I - Implement
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        def traverse(p, q):
            if not p and not q:
                return True

            if not p or not q:
                return False

            if p.val != q.val:
                return False

            return (traverse(p.left, q.left) and
                    traverse(p.right, q.right))

        return traverse(p, q)
```

### R - Run
Example with identical trees:
```
Tree p:    1          Tree q:    1
          / \                   / \
         2   3                 2   3
```
- traverse(1,1): values match, check left and right
- traverse(2,2): values match, check left and right
- traverse(None,None): return True, traverse(None,None): return True
- traverse(3,3): values match, check left and right
- traverse(None,None): return True, traverse(None,None): return True
- All return True, final result: True

Example with different trees:
```
Tree p:    1          Tree q:    1
          / \                   / \
         2   3                 2   4
```
- traverse(1,1): values match, check left and right
- traverse(2,2): return True
- traverse(3,4): values don't match, return False
- Final result: False

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the smaller tree
- Each node is visited exactly once
- If one tree is much smaller, we still visit all nodes in the smaller tree
- Total: O(n)

Space Complexity: O(h) where h is the height of the smaller tree
- Recursion stack depth equals the height of the tree
- In worst case (skewed tree): h = n
- In best case (balanced tree): h = log(n)
- Total: O(h)

Key Insight
The key insight is that we can traverse both trees simultaneously and compare nodes at each step. The recursive structure naturally handles the comparison - if structure or values differ at any point, the comparison fails. The base cases handle all edge cases including empty trees.

What I Learned
Concurrent recursive traversal is a clean approach for comparing trees. The pattern of checking base cases first (both empty, one empty, value mismatch) before recursive calls ensures we handle all edge cases properly. This approach is more intuitive than iterative solutions using stacks or queues for tree comparison problems.