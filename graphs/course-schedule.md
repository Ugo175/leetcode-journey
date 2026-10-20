Problem: Course Schedule

Difficulty: Medium

### U —> Understand
We need to determine if all courses can be finished given prerequisites:
- Input: number of courses and list of prerequisite pairs [course, prerequisite]
- Output: True if all courses can be finished, False otherwise
- A course can only be taken if all its prerequisites are completed
- We need to detect if there's a cycle in the prerequisite graph
- Edge cases: no prerequisites, single course, cycle exists, no cycle

### M —> Match
This is a graph cycle detection problem:
- Courses are nodes, prerequisites are directed edges
- A cycle means courses can't be completed (circular dependency)
- We can use DFS with state tracking to detect cycles
- Three states: unvisited, visiting (in current DFS path), visited (fully processed)
- Pattern: DFS with cycle detection using state tracking

### P - Plan
1. Build adjacency list representation of the graph
2. Initialize visited and visiting sets
3. Define DFS function:
   - If course is in visited, return True (already verified)
   - If course is in visiting, return False (cycle detected)
   - Add course to visiting set
   - For each prerequisite, recursively run DFS
   - If any prerequisite returns False, return False
   - Remove course from visiting, add to visited
   - Return True
4. Run DFS on all courses
5. Return True if all courses can be completed, False otherwise

### I - Implement
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = {i: [] for i in range(numCourses)}

        for course, pre in prerequisites:
            graph[course].append(pre)

        visited, visiting = set(), set()

        def dfs(course):
            if course in visited:
                return True

            if course in visiting:
                return False

            visiting.add(course)

            for pre in graph[course]:
                if not dfs(pre):
                    return False

            visiting.remove(course)
            visited.add(course)

            return True

        for course in range(numCourses):
            if not dfs(course):
                return False

        return True
```

### R - Run
Example with numCourses = 2, prerequisites = [[1, 0]]:
- graph = {0: [], 1: [0]}
- course=0: dfs(0) → visiting={0}, no prerequisites, visited={0}, return True
- course=1: dfs(1) → visiting={1}, prerequisite=0, dfs(0) already visited, return True
- visiting={1}, visited={0,1}, return True
- Return True (can finish)

Example with numCourses = 2, prerequisites = [[1, 0], [0, 1]]:
- graph = {0: [1], 1: [0]}
- course=0: dfs(0) → visiting={0}, prerequisite=1, dfs(1) → visiting={0,1}, prerequisite=0, 0 in visiting, return False
- Return False (cycle detected)

### E - Evaluate
Time Complexity: O(V + E) where V = numCourses, E = len(prerequisites)
- Building graph: O(E)
- Each course visited once: O(V)
- Each prerequisite edge processed once: O(E)
- Total: O(V + E)

Space Complexity: O(V + E)
- Graph storage: O(V + E)
- Visited and visiting sets: O(V)
- Recursion stack: O(V) in worst case
- Total: O(V + E)

Key Insight
The key insight is using three states to detect cycles efficiently. "Visiting" tracks nodes in the current DFS path - if we encounter a node in this state, we've found a cycle. "Visited" tracks fully processed nodes - if we encounter these, we skip them since they're already verified as cycle-free. This prevents redundant work and efficiently detects cycles.

What I Learned
Cycle detection in directed graphs requires tracking the current path separately from fully processed nodes. The three-state DFS pattern (unvisited, visiting, visited) is standard for topological sort and cycle detection problems. This approach is more efficient than simple visited/unvisited tracking for cycle detection in directed graphs.