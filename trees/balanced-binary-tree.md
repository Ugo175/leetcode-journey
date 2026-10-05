Problem: Balanced Binary Tree

Difficulty: Easy

### U —> Understand
We need to determine if a binary tree is height-balanced:
- Input: root of a binary tree
- Output: True if balanced, False otherwise
- A tree is balanced if for every node, the height difference between left and right subtrees is at most 1
- Empty tree is considered balanced
- Edge cases: empty tree, single node, skewed tree, perfectly balanced tree

### M —> Match
This is a bottom-up recursive tree problem:
- We can compute height and check balance in a single pass
- Bottom-up approach avoids redundant calculations
- If any subtree is unbalanced, we can propagate this information up
- Pattern: Bottom-up recursion returning both height and balance status

### P - Plan
1. Handle edge case: if root is None, return True
2. Define a recursive height function:
   - Base case: if node is None, return 0
   - Recursively get height of left subtree
   - Recursively get height of right subtree
   - If either subtree returned -1 (unbalanced), propagate -1
   - If height difference > 1, return -1 (unbalanced)
   - Otherwise, return actual height (1 + max of subtree heights)
3. The tree is balanced if height function doesn't return -1

### I - Implement
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True

        def height(node):
            if not node:
                return 0

            left = height(node.left)
            right = height(node.right)

            if left == -1 or right == -1:
                return -1

            if abs(right - left) > 1:
                return -1

            return 1 + max(left, right)

        return height(root) != -1
```

### R - Run
Example with balanced tree:
```
      3
     / \
    9  20
      /  \
     15   7
```
- height(3): left=height(9), right=height(20)
- height(9): left=0, right=0, diff=0, return 1
- height(20): left=height(15), right=height(7)
- height(15): left=0, right=0, diff=0, return 1
- height(7): left=0, right=0, diff=0, return 1
- height(20): left=1, right=1, diff=0, return 2
- height(3): left=1, right=2, diff=1, return 3
- height(3) != -1, return True

Example with unbalanced tree:
```
    1
   /
  2
 /
3
```
- height(1): left=height(2), right=0
- height(2): left=height(3), right=0
- height(3): left=0, right=0, return 1
- height(2): left=1, right=0, diff=1, return 2
- height(1): left=2, right=0, diff=2 > 1, return -1
- height(1) == -1, return False

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the tree
- Each node is visited exactly once
- No redundant calculations (bottom-up approach)
- Total: O(n)

Space Complexity: O(h) where h is the height of the tree
- Recursion stack depth equals the height of the tree
- In worst case (skewed tree): h = n
- In best case (balanced tree): h = log(n)
- Total: O(h)

Key Insight
The key insight is combining height calculation and balance checking in a single recursive function. By returning -1 for unbalanced subtrees, we can propagate this information up efficiently without needing separate height and balance checks. This bottom-up approach is much more efficient than a top-down approach that would recalculate heights multiple times.

What I Learned
Bottom-up recursion is elegant for tree problems that need to compute properties based on subtree results. Using sentinel values (like -1) to indicate special conditions (unbalanced) allows us to combine multiple computations in a single pass. This approach avoids the O(n²) complexity of naive top-down solutions that recalculate subtree heights repeatedly.