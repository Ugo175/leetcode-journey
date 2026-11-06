Problem: Count Servers that Communicate

Difficulty: Hard

### U —> Understand
We need to count inactive servers at specific query times:
- Input: number of servers n, logs (server_id, timestamp), time window x, queries
- Output: count of inactive servers for each query time
- A server is active if it has at least one log in the time window [query_time - x, query_time]
- Edge cases: no logs, time window too small/large, multiple logs from same server

### M —> Match
This is a sliding window problem with frequency counting:
- Sort logs by timestamp for efficient window management
- Sort queries to process in chronological order
- Use two pointers to maintain the relevant time window
- Track active servers using frequency counting
- Pattern: Sliding window with frequency counting for time-based queries

### P - Plan
1. Sort logs by timestamp
2. Sort queries with their original indices
3. Initialize frequency map, active count, and two pointers
4. For each query in sorted order:
   - Expand window: add logs with timestamp ≤ query_time
   - Shrink window: remove logs with timestamp < query_time - x
   - Count inactive servers: n - active count
   - Store result at original index
5. Return results in original query order

### I - Implement
```python
from collections import defaultdict

class Solution:
    def countServers(self, n: int, logs: List[List[int]], x: int, queries: List[int]) -> List[int]:
        logs.sort(key=lambda log: log[1])

        sorted_queries = sorted([(q, i) for i, q in enumerate(queries)])

        freq = defaultdict(int)
        active = 0

        left = 0
        right = 0

        m = len(logs)
        ans = [0] * len(queries)

        for query_time, idx in sorted_queries:
            # Add logs with time <= query_time
            while right < m and logs[right][1] <= query_time:
                server = logs[right][0]
                if freq[server] == 0:
                    active += 1
                freq[server] += 1
                right += 1

            # Remove logs with time < query_time - x
            start_time = query_time - x
            while left < right and logs[left][1] < start_time:
                server = logs[left][0]
                freq[server] -= 1
                if freq[server] == 0:
                    active -= 1
                left += 1

            ans[idx] = n - active

        return ans
```

### R - Run
Example walkthrough with n=3, logs=[[0,1],[1,2],[2,3]], x=2, queries=[2,3]:
- Sorted logs: [[0,1],[1,2],[2,3]]
- Sorted queries: [(2,0),(3,1)]
- Query time=2:
  - Expand: add logs with time ≤2: [0,1],[1,2], active=2
  - Shrink: remove logs with time <0: none
  - Inactive: 3-2=1
- Query time=3:
  - Expand: add logs with time ≤3: [2,3], active=3
  - Shrink: remove logs with time <1: [0,1], active=2
  - Inactive: 3-2=1
- Return [1,1]

### E - Evaluate
Time Complexity: O((n + q) × log(n + q))
- Sorting logs: O(n × log n)
- Sorting queries: O(q × log q)
- Processing: each log processed once with two pointers: O(n + q)
- Total: O((n + q) × log(n + q))

Space Complexity: O(n + q)
- Frequency map: O(n) in worst case
- Sorted queries array: O(q)
- Answer array: O(q)
- Total: O(n + q)

Key Insight
The key insight is processing queries in chronological order and using a sliding window to maintain the relevant time window. By sorting both logs and queries, we can efficiently add and remove logs as we move through time. The frequency count tracks active servers, and we derive inactive count by subtracting from total servers.

What I Learned
Sliding window with two pointers is powerful for time-based queries. Processing in sorted order allows efficient window management without recomputing for each query. The pattern of "sort once, then process with sliding window" is common for range query problems. Frequency counting with decrement logic ensures we correctly track when servers become inactive.