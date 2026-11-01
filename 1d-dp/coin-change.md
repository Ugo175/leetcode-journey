Problem: Coin Change

Difficulty: Medium

### U —> Understand
We need to find the minimum number of coins to make up a given amount:
- Input: array of coin denominations and target amount
- Output: minimum number of coins needed, or -1 if impossible
- We can use each coin denomination unlimited times
- Coins are positive integers
- Edge cases: amount = 0, impossible amount, single coin denomination

### M —> Match
This is a 1D dynamic programming problem:
- The problem has optimal substructure (minimum coins for amount n depends on minimum coins for smaller amounts)
- There are overlapping subproblems (same sub-amounts computed multiple times)
- Recurrence relation: dp[n] = min(dp[n-coin] + 1) for all coins
- Pattern: 1D DP with top-down memoization or bottom-up approach

### P - Plan
**Top-Down Memoization Approach:**
1. Create memo dictionary to store computed results
2. Define DP function:
   - Base case: if amount is 0, return 0 (no coins needed)
   - Base case: if amount is negative, return infinity (impossible)
   - If amount is in memo, return stored result
   - Try each coin: recursively compute dp(amount - coin) + 1
   - Store and return minimum of all attempts
3. Call DP function on target amount
4. Return result if not infinity, otherwise return -1

### I - Implement
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        memo = {}

        def dp(n):
            if n == 0:
                return 0
            if n < 0:
                return float('inf')

            if n in memo:
                return memo[n]

            memo[n] = min(dp(n - coin) + 1 for coin in coins)
            return memo[n]

        ans = dp(amount)
        return ans if ans != float('inf') else -1
```

### R - Run
Example walkthrough with coins = [1, 2, 5], amount = 11:
- dp(11): min(dp(10)+1, dp(9)+1, dp(6)+1)
- dp(10): min(dp(9)+1, dp(8)+1, dp(5)+1)
- dp(9): min(dp(8)+1, dp(7)+1, dp(4)+1)
- dp(8): min(dp(7)+1, dp(6)+1, dp(3)+1)
- dp(7): min(dp(6)+1, dp(5)+1, dp(2)+1)
- dp(6): min(dp(5)+1, dp(4)+1, dp(1)+1)
- dp(5): min(dp(4)+1, dp(3)+1, dp(0)+1) = min(dp(4)+1, dp(3)+1, 1)
- dp(4): min(dp(3)+1, dp(2)+1, dp(-1)+1) = min(dp(3)+1, dp(2)+1, inf)
- dp(3): min(dp(2)+1, dp(1)+1, dp(-2)+1) = min(dp(2)+1, dp(1)+1, inf)
- dp(2): min(dp(1)+1, dp(0)+1, dp(-3)+1) = min(dp(1)+1, 1, inf)
- dp(1): min(dp(0)+1, dp(-1)+1, dp(-4)+1) = min(1, inf, inf) = 1
- dp(2): min(1+1, 1, inf) = 2
- dp(3): min(2+1, 1+1, inf) = 2
- dp(4): min(2+1, 2+1, inf) = 3
- dp(5): min(3+1, 2+1, 1) = 2
- dp(6): min(2+1, 3+1, 1+1) = 2
- dp(7): min(2+1, 2+1, 2+1) = 3
- dp(8): min(3+1, 2+1, 2+1) = 3
- dp(9): min(3+1, 3+1, 3+1) = 3
- dp(10): min(3+1, 3+1, 2+1) = 3
- dp(11): min(3+1, 3+1, 2+1) = 3
- Return 3 (coins: 5 + 5 + 1)

### E - Evaluate
Time Complexity: O(amount × len(coins))
- Number of distinct subproblems: amount + 1
- For each subproblem, we try all coins: len(coins) operations
- Memoization ensures each subproblem computed once
- Total: O(amount × len(coins))

Space Complexity: O(amount)
- Memo dictionary stores up to amount + 1 entries
- Recursion stack depth: O(amount) in worst case
- Total: O(amount)

Key Insight
The key insight is that the minimum coins for amount n can be derived from the minimum coins for smaller amounts. By trying each coin and taking the minimum, we explore all possibilities while avoiding redundant computations through memoization. Using infinity for impossible states simplifies the logic - we just take the minimum and check if the final result is valid.

What I Learned
Top-down memoization is intuitive for DP problems - it follows the natural recursive thinking process. The pattern of "base cases, memo check, recursive computation, store result" is standard. Using sentinel values (like infinity) for impossible states makes the logic cleaner. This problem demonstrates how DP transforms exponential brute force into polynomial time through memoization.