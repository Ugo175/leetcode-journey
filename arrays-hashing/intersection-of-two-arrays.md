Problem: Intersection of Two Arrays

Difficulty: Easy

### U —> Understand
We need to find the intersection of two arrays:
- Input: two arrays of integers
- Output: array of unique elements that appear in both arrays
- The result can be in any order
- Each element in the result must be unique
- Edge cases: no intersection, one array empty, all elements same, duplicates in input arrays

### M —> Match
This is a hash set problem because:
- We need to find common elements between two arrays
- Sets provide O(1) lookup and efficient intersection operations
- We can convert arrays to sets and use set intersection
- Pattern: Using sets for finding common elements

### P — Plan
1. Convert both arrays to sets to remove duplicates
2. Use set intersection operation (&) to find common elements
3. Convert the result back to a list
4. Return the list of intersecting elements

### I - Implement
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

### R - Run
Example walkthrough with nums1 = [1, 2, 2, 1], nums2 = [2, 2]:
- set(nums1) = {1, 2}
- set(nums2) = {2}
- {1, 2} & {2} = {2}
- list({2}) = [2]
- Return [2]

Example with nums1 = [4, 9, 5], nums2 = [9, 4, 9, 8, 4]:
- set(nums1) = {4, 9, 5}
- set(nums2) = {9, 4, 8}
- {4, 9, 5} & {9, 4, 8} = {4, 9}
- Return [4, 9] (order may vary)

### E - Evaluate
Time Complexity: O(n + m) where n = len(nums1), m = len(nums2)
- Converting nums1 to set: O(n)
- Converting nums2 to set: O(m)
- Set intersection: O(min(n, m))
- Converting result to list: O(k) where k is intersection size
- Total: O(n + m)

Space Complexity: O(n + m)
- Storing set for nums1: O(n)
- Storing set for nums2: O(m)
- Result list: O(k)
- Total: O(n + m)

Key Insight
The key insight is that sets automatically handle duplicates and provide efficient intersection operations. By converting both arrays to sets, we leverage Python's built-in set operations which are optimized for this exact use case. This is much cleaner and more efficient than manual iteration and comparison.

What I Learned
Set operations are powerful for problems involving finding common elements, duplicates, or unique values. The pattern of "convert to sets, perform operation, convert back" is common and readable. While this approach uses extra space, the time complexity improvement over nested loops (O(n×m) to O(n+m)) is significant.