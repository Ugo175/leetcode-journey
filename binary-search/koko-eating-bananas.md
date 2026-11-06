Problem: Koko Eating Bananas

Difficulty: Medium

### U —> Understand
We need to find the minimum eating speed for Koko to finish all bananas:
- Input: array of banana piles and hours h
- Output: minimum integer eating speed (bananas per hour)
- Koko can eat at most k bananas per hour from one pile
- If a pile has fewer than k bananas, she finishes that hour and doesn't eat from other piles
- Edge cases: single pile, all piles same size, speed must be at least 1

### M —> Match
This is a binary search problem:
- We need to find the minimum speed that allows finishing within h hours
- Higher speed always means fewer hours (monotonic relationship)
- Binary search on the speed to find the minimum valid speed
- Pattern: Binary search for minimum value satisfying a condition

### P - Plan
1. Calculate bounds:
   - Left bound: minimum possible speed = ceil(sum(piles) / h)
   - Right bound: maximum pile size (guaranteed to work)
2. Binary search between bounds:
   - Calculate mid speed
   - Calculate hours needed at this speed
   - If hours ≤ h: search left for smaller valid speed
   - If hours > h: search right for larger speed
3. Return left (minimum valid speed)

### I - Implement
```python
class Solution:
    def minEatingSpeed(self, piles: list[int], h: int) -> int:
        left = (sum(piles) + h - 1) // h
        right = max(piles)

        while left <= right:
            mid = (left + right) // 2

            hours = 0
            for pile in piles:
                hours += (pile + mid - 1) // mid

            if hours <= h:
                right = mid - 1
            else:
                left = mid + 1

        return left
```

### R - Run
Example walkthrough with piles = [3, 6, 7, 11], h = 8:
- left = ceil(27/8) = 4, right = 11
- mid = 7: hours = ceil(3/7) + ceil(6/7) + ceil(7/7) + ceil(11/7) = 1+1+1+2 = 5 ≤ 8, right = 6
- mid = 5: hours = ceil(3/5) + ceil(6/5) + ceil(7/5) + ceil(11/5) = 1+2+2+3 = 8 ≤ 8, right = 4
- mid = 4: hours = ceil(3/4) + ceil(6/4) + ceil(7/4) + ceil(11/4) = 1+2+2+3 = 8 ≤ 8, right = 3
- left = 4 > right = 3, exit loop
- Return 4

### E - Evaluate
Time Complexity: O(n × log(max_pile))
- Binary search: O(log(max_pile)) iterations
- Each iteration: O(n) to calculate hours
- Total: O(n × log(max_pile))

Space Complexity: O(1)
- Only using a few variables
- No extra data structures needed
- Total: O(1)

Key Insight
The key insight is the monotonic relationship between speed and hours: higher speed always means fewer hours. This allows binary search. We can calculate bounds efficiently: minimum speed is total bananas divided by hours (rounded up), maximum speed is the largest pile (guaranteed to work in one hour per pile).

What I Learned
Binary search can be applied to optimization problems where we have a monotonic relationship. The pattern of "find minimum value that satisfies condition" using binary search is powerful. Computing bounds carefully is important - too narrow and we might miss the answer, too wide and we waste iterations. The ceiling division formula (n + d - 1) // d is a useful trick for integer math.