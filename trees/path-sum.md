Problem: Path Sum

Difficulty: Easy

### U —> Understand
We need to determine if there's a root-to-leaf path that sums to a target value:
- Input: root of a binary tree and target sum
- Output: True if such a path exists, False otherwise
- A path must go from root to a leaf node (node with no children)
- The sum is calculated by adding all node values along the path
- Edge cases: empty tree, single node tree, negative values, multiple paths

### M —> Match
This is a recursive tree traversal with target sum tracking:
- We can use DFS to traverse all root-to-leaf paths
- At each node, we subtract the node's value from the remaining target
- When we reach a leaf, we check if the remaining target equals the leaf's value
- Pattern: DFS with running sum/remainder for path problems

### P - Plan
1. Handle edge case: if root is None, return False
2. Define a recursive helper function that takes a node and remaining sum
3. Base cases:
   - If node is None, return False
   - If node is a leaf (no children), check if remaining equals node value
4. Recursive case:
   - Subtract current node's value from remaining sum
   - Recursively check left subtree with updated remaining
   - Recursively check right subtree with updated remaining
   - Return True if either subtree has a valid path
5. Call the helper function on root with target sum

### I - Implement
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False

        def traverse(node, remaining):
            if not node:
                return False

            if not node.right and not node.left:
                return remaining == node.val

            return (traverse(node.left, remaining - node.val) or
                    traverse(node.right, remaining - node.val))

        return traverse(root, targetSum)
```

### R - Run
Example with tree:
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```
targetSum = 22
- traverse(5, 22): check left and right
- traverse(4, 17): check left and right
- traverse(11, 13): check left and right
- traverse(7, 2): leaf, 2 != 7, return False
- traverse(2, 2): leaf, 2 == 2, return True
- Path found: 5 → 4 → 11 → 2 (sum = 22)

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the tree
- In the worst case, we visit every node
- Each node is processed exactly once
- Total: O(n)

Space Complexity: O(h) where h is the height of the tree
- Recursion stack depth equals the height of the tree
- In worst case (skewed tree): h = n
- In best case (balanced tree): h = log(n)
- Total: O(h)

Key Insight
The key insight is that we can reduce the problem at each step by subtracting the current node's value from the target. When we reach a leaf, we simply check if the remaining target equals the leaf's value. This transforms the problem from "find a path that sums to target" to "find a path where the remaining sum equals the leaf value."

What I Learned
DFS with running sums is a powerful pattern for path problems in trees. The approach of subtracting the current value from the target at each step simplifies the base case check. Using OR logic (checking if either subtree has a valid path) efficiently explores all possible paths without needing to track the actual path, just whether one exists.