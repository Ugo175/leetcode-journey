Problem: Subarrays Distinct Element Sum of Squares

Difficulty: Medium

### U —> Understand
We need to calculate the sum of squares of distinct counts for all subarrays:
- Input: array of integers
- Output: sum of (count of distinct elements)² for all subarrays
- A subarray is a contiguous part of the array
- We need to consider all possible subarrays
- Edge cases: all same elements, all distinct elements, single element

### M —> Match
This is a brute force array problem with hash set:
- We need to examine all subarrays
- For each subarray, count distinct elements using a set
- Square the count and add to total
- Pattern: Brute force enumeration with hash set for distinct counting

### P - Plan
1. Initialize sum_squares to 0
2. For each starting index i:
   - Create an empty set to track distinct elements
   - For each ending index j from i to end:
     - Add nums[j] to the set
     - Get distinct count (set size)
     - Add distinct² to sum_squares
3. Return sum_squares

### I - Implement
```python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        sum_squares = 0
        n = len(nums)

        for i in range(n):
            seen = set()
            for j in range(i, n):
                seen.add(nums[j])
                distinct = len(seen)
                sum_squares += distinct * distinct

        return sum_squares
```

### R - Run
Example walkthrough with nums = [1, 2, 1]:
- i=0: seen={}, j=0: seen={1}, distinct=1, sum=1
- i=0: j=1: seen={1,2}, distinct=2, sum=1+4=5
- i=0: j=2: seen={1,2}, distinct=2, sum=5+4=9
- i=1: seen={}, j=1: seen={2}, distinct=1, sum=9+1=10
- i=1: j=2: seen={2,1}, distinct=2, sum=10+4=14
- i=2: seen={}, j=2: seen={1}, distinct=1, sum=14+1=15
- Return 15

### E - Evaluate
Time Complexity: O(n²) where n is the length of the array
- Outer loop: n iterations
- Inner loop: n-i iterations for each i
- Set operations are O(1) on average
- Total: O(n²)

Space Complexity: O(n)
- Set can contain at most n distinct elements
- Only using constant extra space besides the set
- Total: O(n)

Key Insight
The key insight is that we need to examine all subarrays and count distinct elements in each. The set naturally handles the distinct counting, and by building it incrementally as we extend the subarray, we efficiently track distinct elements without recounting.

What I Learned
Brute force enumeration is sometimes the most straightforward approach, especially when the problem size allows it. The pattern of "for each starting point, extend ending point and track state" is common for subarray problems. While this solution is O(n²), it's acceptable for the given constraints. More optimized solutions might use sliding window or more sophisticated data structures.