Problem: Largest Number At Least Twice of Others

Difficulty: Easy

### U - Understand
We need to find if the largest element is at least twice as much as every other number:
- Input: array of non-negative integers
- Output: index of the largest element if it's at least twice as much as every other number, -1 otherwise
- The largest element can only be at one index (no duplicates for the maximum)
- Edge cases: single element array, all elements equal, largest element not dominant

### M - Match
This is a simple iteration problem because:
- We need to find the maximum element and its index
- We need to check if this maximum is at least twice every other element
- No complex data structures needed - just iteration and comparison
- Pattern: Find maximum with validation check

### P - Plan
1. Find the largest element in the array and its index
2. Iterate through the array:
   - For each element (except the largest), check if 2 × element > largest
   - If any element violates this condition, return -1
3. If we complete the loop without violations, return the index of the largest element

### I - Implement
```python
class Solution:
    def dominantIndex(self, nums: List[int]) -> int:
        largest_element = max(nums)
        index_largest = 0

        for i in range(len(nums)):
            if nums[i] == largest_element:
                index_largest = i
            elif (nums[i] * 2) > largest_element:
                return -1

        return index_largest
```

### R - Run
Example walkthrough with nums = [3, 6, 1, 0]:
- largest_element = 6, index_largest = 1
- i=0: nums[0]=3, 3×2=6 > 6? No (6 == 6)
- i=1: nums[1]=6 == largest, index_largest = 1
- i=2: nums[2]=1, 1×2=2 > 6? No
- i=3: nums[3]=0, 0×2=0 > 6? No
- Return 1

Example with nums = [1, 2, 3, 4]:
- largest_element = 4, index_largest = 3
- i=0: nums[0]=1, 1×2=2 > 4? No
- i=1: nums[1]=2, 2×2=4 > 4? No (4 == 4)
- i=2: nums[2]=3, 3×2=6 > 4? Yes, return -1

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- Finding max: O(n)
- Iterating through array: O(n)
- Total: O(n)

Space Complexity: O(1)
- Only using a few variables (largest_element, index_largest, i)
- No extra data structures needed

Key Insight
The key insight is that we don't need to actually store the index of the largest element separately - we can find it during the iteration. When we encounter the largest element, we update the index. For all other elements, we check the dominance condition. This single-pass approach is efficient and clean.

What I Learned
Simple iteration problems can often be solved in a single pass by carefully tracking what we need. The pattern of "find max with condition check" is common and can often be optimized by combining operations. In this case, finding the max and checking the dominance condition can be done together rather than separately.