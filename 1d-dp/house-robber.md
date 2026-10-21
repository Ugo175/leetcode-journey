Problem: House Robber

Difficulty: Medium

### U —> Understand
We need to find the maximum amount of money that can be robbed without alerting the police:
- Input: array of non-negative integers representing money in each house
- Output: maximum amount that can be robbed
- Cannot rob adjacent houses (they have security systems)
- Edge cases: empty array, single house, two houses, all houses

### M —> Match
This is a 1D dynamic programming problem:
- At each house, we have two choices: rob it or skip it
- If we rob house i, we cannot rob house i-1, but can add house i-2
- If we skip house i, we take the best from house i-1
- Recurrence relation: dp[i] = max(nums[i] + dp[i-2], dp[i-1])
- Pattern: 1D DP with two-state decision at each step

### P - Plan
**Bottom-Up Approach:**
1. Handle edge case: if only one house, return its value
2. Initialize dp array of size n
3. Set base cases: dp[0] = nums[0], dp[1] = max(nums[0], nums[1])
4. Iterate from house 2 to n-1:
   - dp[i] = max(nums[i] + dp[i-2], dp[i-1])
   - Either rob current house (add to dp[i-2]) or skip it (take dp[i-1])
5. Return dp[n-1] (maximum amount)

### I - Implement
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)

        if n == 1:
            return nums[0]

        dp = [0] * n

        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])

        for i in range(2, n):
            dp[i] = max(nums[i] + dp[i-2], dp[i-1])

        return dp[-1]
```

### R - Run
Example walkthrough with nums = [2, 7, 9, 3, 1]:
- n = 5, dp = [0, 0, 0, 0, 0]
- dp[0] = 2
- dp[1] = max(2, 7) = 7
- i=2: dp[2] = max(9 + dp[0], dp[1]) = max(9+2, 7) = max(11, 7) = 11
- i=3: dp[3] = max(3 + dp[1], dp[2]) = max(3+7, 11) = max(10, 11) = 11
- i=4: dp[4] = max(1 + dp[2], dp[3]) = max(1+11, 11) = max(12, 11) = 12
- Return 12

Verification: Rob houses 0, 2, 4: 2 + 9 + 1 = 12 ✓

### E - Evaluate
Time Complexity: O(n) where n is the number of houses
- Single pass through the array
- Each iteration does constant work
- Total: O(n)

Space Complexity: O(n)
- DP array stores n values
- Could be optimized to O(1) by only keeping previous two values
- Total: O(n)

Key Insight
The key insight is the two-state decision at each house: rob or skip. If we rob the current house, we must skip the previous one but can add the solution from two houses back. If we skip the current house, we take the best solution from the previous house. This recurrence relation captures all possibilities while maintaining optimal substructure.

What I Learned
DP problems often involve making optimal choices at each step based on previous subproblems. The pattern of "max(include current, exclude current)" is common in optimization problems with constraints. Space optimization is possible here too - we only need the previous two DP values, similar to climbing stairs. Understanding the recurrence relation is the key to solving DP problems.