Problem: Palindrome Path

Difficulty: Hard

### U —> Understand
We need to process queries on a tree to check if paths can form palindromes:
- Input: tree structure (n nodes, edges), string s for each node, queries
- Output: array of boolean answers for each query
- Queries: update node character, or check if path between two nodes can form palindrome
- A path can form palindrome if at most one character appears odd number of times
- Edge cases: single node path, update operations, multiple queries

### M —> Match
This is an advanced graph problem with heavy-light decomposition:
- Need efficient path queries on a tree
- Use heavy-light decomposition to break tree into chains
- Segment tree for XOR operations on character frequencies
- Bitmask to track character parity (odd/even counts)
- Pattern: Heavy-light decomposition + segment tree for tree path queries

### P - Plan
1. Build adjacency list representation of the tree
2. Perform DFS to compute parent, depth, subtree sizes, and heavy children
3. Apply heavy-light decomposition to break tree into chains
4. Convert string to bitmask representation (each character as bit)
5. Build segment tree for XOR operations on the linearized tree
6. For path queries:
   - Use heavy-light decomposition to traverse chains
   - Query segment tree for XOR along path
   - Check if result has at most one bit set (palindrome condition)
7. For update operations:
   - Update character and corresponding bitmask in segment tree

### I - Implement
```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self._build(1, 0, self.n - 1, arr)

    def _build(self, node, l, r, arr):
        if l == r:
            self.tree[node] = arr[l]
            return

        mid = (l + r) // 2
        self._build(node * 2, l, mid, arr)
        self._build(node * 2 + 1, mid + 1, r, arr)
        self.tree[node] = self.tree[node * 2] ^ self.tree[node * 2 + 1]

    def update(self, node, l, r, idx, val):
        if l == r:
            self.tree[node] = val
            return

        mid = (l + r) // 2
        if idx <= mid:
            self.update(node * 2, l, mid, idx, val)
        else:
            self.update(node * 2 + 1, mid + 1, r, idx, val)
        self.tree[node] = self.tree[node * 2] ^ self.tree[node * 2 + 1]

    def query(self, node, l, r, ql, qr):
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.tree[node]
        mid = (l + r) // 2
        return (self.query(node * 2, l, mid, ql, qr) ^ 
                self.query(node * 2 + 1, mid + 1, r, ql, qr))

class Solution:
    def palindromePath(self, n: int, edges: list[list[int]], s: str, queries: list[str]) -> list[bool]:
        # Build adjacency list
        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)

        # DFS for heavy-light decomposition preprocessing
        parent = [-1] * n
        depth = [0] * n
        heavy = [-1] * n
        size = [0] * n

        def dfs(u, p):
            parent[u] = p
            size[u] = 1
            max_size = 0
            for v in g[u]:
                if v == p:
                    continue
                depth[v] = depth[u] + 1
                dfs(v, u)
                size[u] += size[v]
                if size[v] > max_size:
                    max_size = size[v]
                    heavy[u] = v
        dfs(0, -1)

        # Heavy-light decomposition
        head = [0] * n
        pos = [0] * n
        cur_pos = 0

        def decompose(u, h):
            nonlocal cur_pos
            head[u] = h
            pos[u] = cur_pos
            cur_pos += 1
            if heavy[u] != -1:
                decompose(heavy[u], h)
            for v in g[u]:
                if v == parent[u] or v == heavy[u]:
                    continue
                decompose(v, v)
        decompose(0, 0)

        # Convert string to bitmask representation
        base = [0] * n
        for i, ch in enumerate(s):
            base[pos[i]] = 1 << (ord(ch) - ord("a"))

        seg = SegmentTree(base)
        s = list(s)

        def path_query(u, v):
            res = 0
            while head[u] != head[v]:
                if depth[head[u]] < depth[head[v]]:
                    u, v = v, u
                res ^= seg.query(1, 0, n - 1, pos[head[u]], pos[u])
                u = parent[head[u]]
            if depth[u] > depth[v]:
                u, v = v, u
            res ^= seg.query(1, 0, n - 1, pos[u], pos[v])
            return res

        ans = []
        for q in queries:
            parts = q.split()
            if parts[0] == "update":
                u = int(parts[1])
                c = parts[2]
                s[u] = c
                seg.update(1, 0, n - 1, pos[u], 1 << (ord(c) - ord("a")))
            else:
                u = int(parts[1])
                v = int(parts[2])
                mask = path_query(u, v)
                ans.append(mask == 0 or (mask & (mask - 1)) == 0)
        return ans
```

### R - Run
The solution processes queries efficiently by:
1. Building tree structure with heavy-light decomposition
2. Using segment tree for range XOR queries
3. Checking palindrome condition: mask has at most one bit set
4. Each query processed in O(log²n) time using HLD

### E - Evaluate
Time Complexity: O((n + q) × log²n)
- Building HLD: O(n)
- Building segment tree: O(n)
- Each query: O(log²n) due to HLD and segment tree operations
- Total: O((n + q) × log²n)

Space Complexity: O(n)
- Tree storage: O(n)
- HLD arrays: O(n)
- Segment tree: O(n)
- Total: O(n)

Key Insight
The key insight is using heavy-light decomposition to transform tree path queries into array range queries. By breaking the tree into chains and linearizing them, we can use efficient data structures like segment trees. The bitmask representation allows us to check palindrome conditions in O(1) using the property that a number with at most one bit set is either 0 or a power of 2.

What I Learned
Heavy-light decomposition is a powerful technique for tree path queries. Combining it with segment trees enables efficient range operations on tree paths. Bitmask operations are elegant for tracking character frequencies - XOR operations naturally handle parity (odd/even counts). This problem demonstrates how advanced data structures can be combined to solve complex tree query problems efficiently.