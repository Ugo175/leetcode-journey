Problem: Maximum Depth of Binary Tree

Difficulty: Easy

### U —> Understand
We need to find the maximum depth of a binary tree:
- Input: root of a binary tree
- Output: integer representing the maximum depth (number of nodes along the longest path from root to leaf)
- Depth is defined as the number of nodes along the longest path from root to farthest leaf
- An empty tree has depth 0
- Edge cases: empty tree, single node, unbalanced tree, perfectly balanced tree

### M —> Match
This is a recursive tree traversal problem:
- We can use DFS (Depth-First Search) to traverse the tree
- For each node, the depth is 1 + max(depth of left subtree, depth of right subtree)
- Base case: if node is None, return 0
- Pattern: Recursive DFS for tree depth/height calculations

### P - Plan
1. Define a recursive helper function
2. Base case: if node is None, return 0
3. Recursive case: return 1 + max(depth of left child, depth of right child)
4. Call the helper function on the root
5. Return the result

### I - Implement
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def traverse(node):
            if not node:
                return 0
            return 1 + max(traverse(node.left), traverse(node.right))

        return traverse(root)
```

### R - Run
Example with tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
- traverse(3): 1 + max(traverse(9), traverse(20))
- traverse(9): 1 + max(traverse(None), traverse(None)) = 1 + max(0, 0) = 1
- traverse(20): 1 + max(traverse(15), traverse(7))
- traverse(15): 1 + max(0, 0) = 1
- traverse(7): 1 + max(0, 0) = 1
- traverse(20): 1 + max(1, 1) = 2
- traverse(3): 1 + max(1, 2) = 3
- Return 3

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the tree
- Each node is visited exactly once
- Each visit involves constant time operations
- Total: O(n)

Space Complexity: O(h) where h is the height of the tree
- Recursion stack depth equals the height of the tree
- In worst case (skewed tree): h = n
- In best case (balanced tree): h = log(n)
- Total: O(h)

Key Insight
The key insight is that the depth of any node is 1 plus the maximum depth of its children. This recursive relationship naturally leads to a DFS solution. The base case handles null nodes (depth 0), and the recursive case builds up the depth by taking the maximum of the two subtrees.

What I Learned
Recursive tree traversal is elegant for depth/height calculations. The pattern of "1 + max(recursive calls)" is standard for finding maximum depth. Understanding the space complexity based on tree height (recursion stack depth) is important - balanced trees are more space-efficient than skewed trees for recursive solutions.