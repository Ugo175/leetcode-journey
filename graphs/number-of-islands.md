Problem: Number of Islands

Difficulty: Medium

### U —> Understand
We need to count the number of islands in a 2D grid:
- Input: 2D grid of '1's (land) and '0's (water)
- Output: number of islands (connected groups of '1's)
- An island is surrounded by water and formed by connecting adjacent lands horizontally or vertically
- Edge cases: empty grid, all water, all land, single cell
- Grid dimensions: m rows × n columns

### M —> Match
This is a graph traversal problem on a grid:
- Each '1' is a node, adjacent '1's are connected by edges
- We need to find connected components in this graph
- BFS or DFS can be used to explore each island
- We mark visited cells to avoid counting them multiple times
- Pattern: BFS/DFS on grid for connected components

### P - Plan
1. Handle edge case: if grid is empty, return 0
2. Initialize island count and directions (up, down, left, right)
3. Iterate through each cell in the grid:
   - If cell is '1' (unvisited land):
     - Increment island count
     - Mark cell as visited (change to '-1')
     - Start BFS from this cell to mark all connected land
4. BFS for island exploration:
   - Use queue to process cells
   - For each cell, check all 4 neighbors
   - If neighbor is valid and is '1', mark as visited and add to queue
5. Return total island count

### I - Implement
```python
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        row, col = len(grid), len(grid[0])
        no_islands = 0
        dirs = [(1, 0), (0, 1), (-1, 0), (0, -1)]

        for r in range(row):
            for c in range(col):
                if grid[r][c] == '1':
                    no_islands += 1
                    grid[r][c] = '-1'
                    queue = deque([(r, c)])

                    while queue:
                        x, y = queue.popleft()
                        for n, a in dirs:
                            nx, ny = x + n, y + a
                            if 0 <= nx < row and 0 <= ny < col and grid[nx][ny] == '1':
                                grid[nx][ny] = '-1'
                                queue.append((nx, ny))

        return no_islands
```

### R - Run
Example with grid:
```
[
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
- (0,0): '1', count=1, BFS marks (0,0),(0,1),(1,0),(1,1) as visited
- (0,2): '0', skip
- (0,3): '0', skip
- (0,4): '0', skip
- (1,2): '0', skip
- (1,3): '0', skip
- (1,4): '0', skip
- (2,0): '0', skip
- (2,1): '0', skip
- (2,2): '1', count=2, BFS marks (2,2) as visited
- (2,3): '0', skip
- (2,4): '0', skip
- (3,0): '0', skip
- (3,1): '0', skip
- (3,2): '0', skip
- (3,3): '1', count=3, BFS marks (3,3),(3,4) as visited
- Return 3

### E - Evaluate
Time Complexity: O(m × n) where m = rows, n = columns
- Each cell is visited at most once
- Each cell is processed with constant time operations
- Total: O(m × n)

Space Complexity: O(m × n) in the worst case
- Queue can contain up to all cells in the worst case (single large island)
- Grid modification is in-place
- Total: O(m × n)

Key Insight
The key insight is treating the grid as a graph where each land cell is a node and adjacent land cells are connected. By using BFS to explore each connected component (island) and marking visited cells, we can count the number of islands efficiently. The marking prevents us from counting the same land multiple times.

What I Learned
Grid problems can often be modeled as graph problems. BFS/DFS on grids follows the same pattern as on graphs - we just need to handle boundary checks. The pattern of "find unvisited node, start traversal, mark visited" is standard for connected components problems. In-place modification to mark visited cells saves space compared to using a separate visited array.