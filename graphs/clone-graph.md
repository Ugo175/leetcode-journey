Problem: Clone Graph

Difficulty: Medium

### U —> Understand
We need to create a deep copy of a connected undirected graph:
- Input: a node from a connected undirected graph
- Output: a deep copy of the graph (new nodes with same structure and values)
- The graph is represented using adjacency list via Node class
- Each Node has a value and list of neighbors
- We need to clone all nodes and their connections
- Edge cases: single node, node with no neighbors, complex graph structure

### M —> Match
This is a graph traversal problem with cloning:
- We need to visit all nodes in the original graph
- We use a hashmap to map original nodes to their clones
- DFS or BFS can be used for traversal
- Pattern: Graph traversal with hashmap for cloning/copying

### P - Plan
1. Handle edge case: if node is None, return None
2. Create a hashmap to map original nodes to their clones
3. Define DFS function:
   - If node is already in hashmap, return its clone (already copied)
   - Create a copy of the current node
   - Add mapping to hashmap
   - For each neighbor, recursively clone and add to copy's neighbors
   - Return the cloned node
4. Start DFS from the given node
5. Return the cloned graph

### I - Implement
```python
from typing import Optional

# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if not node:
            return None

        hashmap = {}

        def dfs(node):
            if node in hashmap:
                return hashmap[node]

            copy = Node(node.val)
            hashmap[node] = copy

            for neighbor in node.neighbors:
                copy.neighbors.append(dfs(neighbor))

            return copy

        return dfs(node)
```

### R - Run
Example walkthrough with graph: 1 — 2 — 3, and 1 — 4
- Start with node 1
- dfs(1): create copy(1), hashmap = {1: copy(1)}
- neighbors of 1: [2, 4]
- dfs(2): create copy(2), hashmap = {1: copy(1), 2: copy(2)}
- neighbors of 2: [1, 3]
- dfs(1): already in hashmap, return copy(1)
- dfs(3): create copy(3), hashmap = {..., 3: copy(3)}
- neighbors of 3: [2]
- dfs(2): already in hashmap, return copy(2)
- copy(2).neighbors = [copy(1), copy(3)]
- dfs(4): create copy(4), hashmap = {..., 4: copy(4)}
- neighbors of 4: [1]
- dfs(1): already in hashmap, return copy(1)
- copy(4).neighbors = [copy(1)]
- copy(1).neighbors = [copy(2), copy(4)]
- Return copy(1)

### E - Evaluate
Time Complexity: O(V + E) where V = number of vertices, E = number of edges
- Each node is visited exactly once
- Each edge is processed exactly once
- Total: O(V + E)

Space Complexity: O(V)
- Hashmap stores V entries (original → clone mapping)
- Recursion stack depth: O(V) in worst case
- Total: O(V)

Key Insight
The key insight is using a hashmap to track which nodes have already been cloned. This prevents infinite loops in cyclic graphs and ensures we don't create duplicate clones. The hashmap serves as both a visited set and a mapping for connecting the cloned graph correctly.

What I Learned
Graph cloning requires careful tracking of original-to-copy mappings. The pattern of "check if already cloned, create clone, recursively clone neighbors, return clone" handles all cases including cycles. Using recursion with memoization (hashmap) is elegant for graph problems where we need to preserve structure while creating copies.