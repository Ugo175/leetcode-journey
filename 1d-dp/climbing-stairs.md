Problem: Climbing Stairs

Difficulty: Easy

### U —> Understand
We need to find the number of distinct ways to climb to the top of stairs:
- Input: integer n representing the number of stairs
- Output: number of distinct ways to reach the top
- You can climb either 1 or 2 steps at a time
- Edge cases: n = 0, n = 1, n = 2, large n

### M —> Match
This is a 1D dynamic programming problem:
- The problem has optimal substructure (ways to reach step n depends on ways to reach n-1 and n-2)
- There are overlapping subproblems (same subproblems solved multiple times)
- Recurrence relation: dp[n] = dp[n-1] + dp[n-2]
- This is essentially the Fibonacci sequence
- Pattern: 1D DP with recurrence relation

### P - Plan
**Optimized Bottom-Up Approach:**
1. Handle base cases: if n ≤ 1, return 1
2. Use only the previous two values (we don't need full array)
3. Initialize prev2 = 1 (ways to reach step 0), prev = 1 (ways to reach step 1)
4. Iterate from step 2 to n:
   - Calculate current = prev + prev2
   - Update prev2 = prev, prev = current
5. Return prev (which holds the result for step n)

### I - Implement
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return 1

        current, prev, prev2 = 0, 1, 1

        for i in range(2, n + 1):
            current = prev + prev2
            prev2, prev = prev, current

        return prev
```

### R - Run
Example walkthrough with n = 3:
- Base case: n > 1, so proceed
- prev2 = 1 (step 0), prev = 1 (step 1)
- i=2: current = 1 + 1 = 2, prev2 = 1, prev = 2
- i=3: current = 2 + 1 = 3, prev2 = 2, prev = 3
- Return 3

Verification: Ways to climb 3 stairs:
- 1+1+1, 1+2, 2+1 = 3 ways ✓

### E - Evaluate
Time Complexity: O(n) where n is the number of stairs
- Single loop from 2 to n
- Each iteration does constant work
- Total: O(n)

Space Complexity: O(1)
- Only using three variables (current, prev, prev2)
- No array or extra data structures needed
- Total: O(1)

Key Insight
The key insight is that to reach step n, you can either come from step n-1 (taking 1 step) or step n-2 (taking 2 steps). This gives us the recurrence dp[n] = dp[n-1] + dp[n-2]. Since we only need the previous two values at any point, we can optimize space from O(n) to O(1) by not storing the entire DP array.

What I Learned
Dynamic programming problems often follow the pattern of identifying the recurrence relation first. The Fibonacci-style recurrence (sum of previous two states) is common. Space optimization in DP is achieved by recognizing that we often only need a subset of previous states, not the entire history. The optimized bottom-up approach is both time and space efficient for this problem.