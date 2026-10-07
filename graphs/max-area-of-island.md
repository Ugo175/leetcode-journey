Problem: Max Area of Island

Difficulty: Medium

### U —> Understand
We need to find the maximum area of an island in a 2D grid:
- Input: 2D grid of 1s (land) and 0s (water)
- Output: maximum area of any island
- Area is the number of cells in the island
- An island is connected 1s horizontally or vertically
- Edge cases: empty grid, no islands, single cell islands, one large island
- Grid dimensions: m rows × n columns

### M —> Match
This is a graph traversal problem with area calculation:
- Similar to Number of Islands, but we need to calculate area instead of just counting
- BFS or DFS to explore each island and count its cells
- Track the maximum area found during exploration
- Pattern: BFS/DFS on grid with area counting

### P - Plan
1. Initialize max area to 0 and directions (up, down, left, right)
2. Iterate through each cell in the grid:
   - If cell is 1 (unvisited land):
     - Start BFS from this cell
     - Initialize area count to 1
     - Mark cell as visited (change to -1)
     - BFS to explore all connected land:
       - For each cell, check all 4 neighbors
       - If neighbor is valid and is 1, mark as visited, add to queue, increment area
     - Update max area if current island area is larger
3. Return max area

### I - Implement
```python
from collections import deque

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        rowLength, columnLength = len(grid), len(grid[0])
        directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]
        maxArea = 0

        for row in range(rowLength):
            for col in range(columnLength):
                if grid[row][col] == 1:
                    queue = deque([(row, col)])
                    areaCount = 1
                    grid[row][col] = -1

                    while queue:
                        x, y = queue.popleft()
                        for r, c in directions:
                            nx, ny = x + r, y + c
                            if 0 <= nx < rowLength and 0 <= ny < columnLength and grid[nx][ny] == 1:
                                queue.append((nx, ny))
                                areaCount += 1
                                grid[nx][ny] = -1

                    maxArea = max(maxArea, areaCount)

        return maxArea
```

### R - Run
Example with grid:
```
[
  [0,0,1,0,0,0,0,1,0,0,0,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,1,1,0,1,0,0,0,0,0,0,0,0],
  [0,1,0,0,1,1,0,0,1,0,1,0,0],
  [0,1,0,0,1,1,0,0,1,1,1,0,0],
  [0,0,0,0,0,0,0,0,0,0,1,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
```
- Find island at (0,7): area = 1, maxArea = 1
- Find island at (1,7): BFS explores connected cells, area = 6, maxArea = 6
- Find island at (2,1): BFS explores connected cells, area = 5, maxArea = 6
- Find island at (3,7): BFS explores connected cells, area = 2, maxArea = 6
- Find island at (4,7): BFS explores connected cells, area = 8, maxArea = 8
- Return 6 (or whichever is the actual maximum from the grid)

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
The key insight is that this is essentially the same as Number of Islands, but instead of just counting islands, we count the cells within each island. The BFS exploration pattern is identical - we just need to track the area (count of cells) during exploration and keep the maximum found.

What I Learned
Many grid/graph problems share the same traversal pattern. The difference often lies in what we track during traversal (count vs. area vs. path vs. etc.). The pattern of "find unvisited, traverse, count/track, update maximum" is versatile. In-place modification for marking visited cells is space-efficient and works well for both counting and area calculation problems.