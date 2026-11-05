Problem: Find the Distance Value Between Two Arrays

Difficulty: Easy

### U —> Understand
We need to count elements in arr1 that are "good" relative to arr2:
- Input: two arrays arr1, arr2, and integer d
- Output: count of elements in arr1 where the element is at distance > d from all elements in arr2
- An element is "good" if |arr1[i] - arr2[j]| > d for all j
- Edge cases: empty arrays, single element arrays, all elements good, no elements good

### M —> Match
This is a simple array comparison problem:
- We need to compare each element in arr1 against all elements in arr2
- For each element in arr1, check if it satisfies the distance condition with all elements in arr2
- Pattern: Nested array comparison with distance condition

### P - Plan
1. Initialize count to 0
2. For each element in arr1:
   - Assume it's good initially
   - For each element in arr2:
     - If distance ≤ d, mark as not good and break
   - If still good after checking all arr2 elements, increment count
3. Return count

### I - Implement
```python
class Solution:
    def findTheDistanceValue(self, arr1: list[int], arr2: list[int], d: int) -> int:
        count = 0

        for a in arr1:
            good = True

            for b in arr2:
                if abs(a - b) <= d:
                    good = False
                    break

            if good:
                count += 1

        return count
```

### R - Run
Example walkthrough with arr1 = [4, 5, 8], arr2 = [10, 9, 1, 8], d = 2:
- a=4: check against [10,9,1,8]
  - |4-10|=6 > 2, |4-9|=5 > 2, |4-1|=3 > 2, |4-8|=4 > 2 ✓
  - good = True, count = 1
- a=5: check against [10,9,1,8]
  - |5-10|=5 > 2, |5-9|=4 > 2, |5-1|=4 > 2, |5-8|=3 > 2 ✓
  - good = True, count = 2
- a=8: check against [10,9,1,8]
  - |8-10|=2 ≤ 2 ✗, break
  - good = False
- Return 2

### E - Evaluate
Time Complexity: O(n × m) where n = len(arr1), m = len(arr2)
- For each element in arr1, we check all elements in arr2
- Total comparisons: n × m
- Total: O(n × m)

Space Complexity: O(1)
- Only using a few variables (count, good flag)
- No extra data structures needed
- Total: O(1)

Key Insight
The key insight is that the condition must hold for ALL elements in arr2, so we can break early if we find any violation. This optimization makes the average case better than the worst case. The problem is straightforward - we just need to implement the distance check correctly.

What I Learned
Simple nested loop problems can often be optimized with early termination. The pattern of "assume success, check all conditions, break on failure" is common for validation problems. While this brute force approach works for the given constraints, sorting arr2 and using binary search could optimize this to O(n × log m) for larger inputs.