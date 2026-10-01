Problem: Best Time to Buy and Sell Stock

Difficulty: Easy

### U —> Understand
We need to find the maximum profit from buying and selling a stock:
- Input: array of prices where prices[i] is the price on day i
- Output: maximum profit achievable (if no profit possible, return 0)
- We can only buy once and sell once
- Must buy before selling (buy day < sell day)
- Edge cases: prices only decrease, prices only increase, single day, empty array

### M —> Match
This is a single-pass array problem with tracking:
- We need to track the minimum price seen so far (best buying opportunity)
- We need to track the maximum profit achievable
- As we iterate, we can calculate profit if we sold at current price after buying at minimum seen price
- Pattern: Single pass with tracking minimum and maximum

### P — Plan
1. Initialize min_price to the first day's price
2. Initialize max_profit to 0
3. Iterate through each price:
   - Update min_price if current price is lower (better buying opportunity)
   - Calculate potential profit: current price - min_price
   - Update max_profit if this profit is higher
4. Return max_profit

### I - Implement
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = prices[0]
        max_profit = 0

        for day in prices:
            min_price = min(min_price, day)
            max_profit = max(max_profit, (day - min_price))

        return max_profit
```

### R - Run
Example walkthrough with prices = [7, 1, 5, 3, 6, 4]:
- min_price=7, max_profit=0
- day=7: min_price=7, profit=0, max_profit=0
- day=1: min_price=1, profit=0, max_profit=0
- day=5: min_price=1, profit=4, max_profit=4
- day=3: min_price=1, profit=2, max_profit=4
- day=6: min_price=1, profit=5, max_profit=5
- day=4: min_price=1, profit=3, max_profit=5
- Return 5 (buy at 1, sell at 6)

Example with prices = [7, 6, 4, 3, 1]:
- All prices decreasing, max_profit stays 0
- Return 0

### E - Evaluate
Time Complexity: O(n) where n is the length of prices array
- Single pass through the array
- Each operation is O(1)
- Total: O(n)

Space Complexity: O(1)
- Only using two variables (min_price, max_profit)
- No extra data structures needed

Key Insight
The key insight is that we don't need to know when to sell - we just need to know the minimum price seen so far. For any given day, the best profit if we sell on that day is current price minus the minimum price seen before. By tracking this minimum and updating the maximum profit as we go, we can find the optimal buy-sell pair in a single pass.

What I Learned
Many optimization problems can be solved with single-pass tracking. The pattern of "track minimum so far and calculate some metric" is common for problems involving differences or ranges. This approach is much more efficient than checking all possible pairs (which would be O(n²)).