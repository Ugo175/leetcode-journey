Problem: Contains Duplicate

Difficulty: Easy

### U - Understand
We need to determine if an array contains any duplicate values:
- Input: an array of integers
- Output: True if any value appears at least twice, False otherwise
- Edge cases: empty array, single element, all same elements, all unique elements

### M - Match
This is a hash set problem because:
- Sets automatically remove duplicates
- Comparing set length with array length tells us if duplicates existed
- If lengths differ, duplicates were removed by the set
- Pattern: Using set properties for duplicate detection

### P - Plan
1. Convert the array to a set (which automatically removes duplicates)
2. Compare the length of the set with the length of the original array
3. If lengths are equal, no duplicates exist (return False)
4. If lengths differ, duplicates exist (return True)

### I - Implement
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        if len(set(nums)) - len(nums) == 0:
            return False
        return True
```

### R - Run
Example walkthrough with nums = [1, 2, 3, 1]:
- set(nums) = {1, 2, 3}, len(set) = 3
- len(nums) = 4
- 3 - 4 = -1 ≠ 0, return True

Example with nums = [1, 2, 3, 4]:
- set(nums) = {1, 2, 3, 4}, len(set) = 4
- len(nums) = 4
- 4 - 4 = 0, return False

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- Each lookup and insertion in the set is O(1) on average
- We perform n operations total

Space Complexity: O(n) in the worst case
- In the worst case (all unique elements), the set stores n elements
- Best case: O(1) if duplicate found early

Key Insight
Using a hash set allows us to detect duplicates in a single pass through the array, avoiding the O(n²) time complexity of a nested loop approach.

What I Learned
Hash sets are ideal for membership testing and duplicate detection problems. The trade-off is increased space usage for improved time complexity.
