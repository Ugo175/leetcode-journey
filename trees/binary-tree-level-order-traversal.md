Problem: Binary Tree Level Order Traversal

Difficulty: Medium

### U —> Understand
We need to return the level-order traversal of a binary tree:
- Input: root of a binary tree
- Output: list of lists, where each inner list contains node values at that level
- Level-order means level by level, left to right within each level
- Empty tree returns empty list
- Edge cases: empty tree, single node, unbalanced tree, complete tree

### M —> Match
This is a BFS (Breadth-First Search) problem using a queue:
- We need to process nodes level by level
- Queue naturally handles FIFO order for level-order traversal
- We process all nodes at current level before moving to next level
- Pattern: BFS with queue for level-order tree traversal

### P - Plan
1. Handle edge case: if root is None, return empty list
2. Initialize queue with root node
3. Initialize result list
4. While queue is not empty:
   - Get current level size (number of nodes at this level)
   - Create empty list for current level values
   - Process all nodes at current level:
     - Pop node from queue
     - Add node value to level list
     - Add left child to queue if exists
     - Add right child to queue if exists
   - Add level list to result
5. Return result

### I - Implement
```python
from collections import deque

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        queue = deque([root])
        result = []

        while queue:
            level = []
            level_length = len(queue)

            for _ in range(level_length):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            result.append(level)

        return result
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
- queue = [3], level_length = 1
- Process level 0: node=3, level=[3], add 9 and 20 to queue
- queue = [9, 20], level_length = 2
- Process level 1: node=9, level=[9]; node=20, level=[9,20], add 15 and 7 to queue
- queue = [15, 7], level_length = 2
- Process level 2: node=15, level=[15]; node=7, level=[15,7]
- queue = [], exit loop
- Return [[3], [9, 20], [15, 7]]

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the tree
- Each node is visited exactly once
- Each node is added to and removed from queue exactly once
- Total: O(n)

Space Complexity: O(n) in the worst case
- Queue stores at most one level of nodes
- In worst case (complete tree's last level): O(n/2) = O(n)
- Result list stores all node values: O(n)
- Total: O(n)

Key Insight
The key insight is using the queue's current size to know when we've finished processing a level. By storing the level size before processing, we can process exactly the nodes at the current level without mixing them with nodes from the next level. This level-by-level processing is the essence of BFS.

What I Learned
BFS with a queue is the standard approach for level-order traversal. The pattern of "store level size, process exactly that many nodes" is crucial for maintaining level boundaries. Using deque instead of a regular list for the queue provides O(1) popleft operations, which is important for efficiency. This approach is more intuitive than recursive solutions for level-order problems.