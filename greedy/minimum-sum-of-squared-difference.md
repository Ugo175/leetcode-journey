Problem: Minimum Sum of Squared Difference

Difficulty: Hard

### U —> Understand
We need to minimize the sum of squared differences between two arrays after performing operations:
- Input: two arrays nums1, nums2, and integers k1, k2 (operations allowed on each array)
- Output: minimum possible sum of squared differences after operations
- We can decrease elements in nums1 by k1 total and nums2 by k2 total
- Each operation decreases one element by 1
- We want to minimize sum of (nums1[i] - nums2[i])²
- Edge cases: k1 + k2 can zero out all differences, arrays of different sizes

### M —> Match
This is a binary search + greedy problem:
- We can use binary search to find the optimal threshold for differences
- Greedy allocation to reduce differences as evenly as possible
- The key insight is that we want to reduce the largest differences first
- Pattern: Binary search for optimal threshold + greedy allocation

### P - Plan
1. Calculate absolute differences between corresponding elements
2. If total differences ≤ k1 + k2, return 0 (can zero out all)
3. Use binary search to find the threshold:
   - Search for the maximum value we can reduce all differences to
   - Calculate operations needed to reduce each difference above threshold
   - Adjust search based on whether operations exceed available k
4. Greedy allocation:
   - Reduce all differences above threshold to threshold
   - Distribute remaining operations evenly among differences at threshold
5. Return sum of squares of final differences

### I - Implement
```python
class Solution:
    def minSumSquareDiff(
        self,
        nums1: list[int],
        nums2: list[int],
        k1: int,
        k2: int
    ) -> int:
        diffs = [abs(a - b) for a, b in zip(nums1, nums2)]
        k = k1 + k2

        if sum(diffs) <= k:
            return 0

        left, right = 0, max(diffs)

        while left < right:
            mid = (left + right) // 2

            operations = 0
            for d in diffs:
                if d > mid:
                    operations += d - mid

            if operations > k:
                left = mid + 1
            else:
                right = mid

        threshold = left

        reduced = []
        used = 0

        for d in diffs:
            if d > threshold:
                used += d - threshold
                reduced.append(threshold)
            else:
                reduced.append(d)

        remaining = k - used

        count_threshold = reduced.count(threshold)

        for i in range(len(reduced)):
            if remaining == 0:
                break

            if reduced[i] == threshold:
                reduced[i] -= 1
                remaining -= 1

        return sum(x * x for x in reduced)
```

### R - Run
Example walkthrough with nums1 = [1,2,3,4], nums2 = [2,3,4,5], k1 = 1, k2 = 1:
- diffs = [1,1,1,1], k = 2
- sum(diffs) = 4 > k = 2, so proceed
- Binary search finds threshold = 0
- All differences > 0, reduce all to 0 using 4 operations, but only have 2
- Reduce two differences to 0, two remain at 1
- reduced = [0,0,1,1]
- sum of squares = 0 + 0 + 1 + 1 = 2

### E - Evaluate
Time Complexity: O(n × log(max_diff))
- n = length of arrays
- Binary search over range [0, max_diff]: O(log(max_diff))
- Each binary search iteration processes all differences: O(n)
- Greedy allocation: O(n)
- Total: O(n × log(max_diff))

Space Complexity: O(n)
- Storing differences array: O(n)
- Storing reduced array: O(n)
- Total: O(n)

Key Insight
The key insight is that to minimize the sum of squares, we want to reduce the largest differences first (since squaring amplifies large values). Binary search helps us find the optimal threshold efficiently, and then we greedily allocate remaining operations. This transforms a complex optimization problem into a structured search problem.

What I Learned
Binary search can be applied to optimization problems where we can evaluate feasibility at a given threshold. The pattern of "binary search for threshold + greedy allocation" is powerful for minimization problems. The key is identifying what makes a solution "feasible" at a given threshold and how to evaluate it efficiently.