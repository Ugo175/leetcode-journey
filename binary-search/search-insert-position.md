Problem: Search Insert Position

Difficulty: Easy

### U —> Understand
We need to find the index where a target should be inserted in a sorted array:
- Input: sorted array of distinct integers and a target integer
- Output: the index where target would be inserted to maintain sorted order
- If target exists, return its index
- If target doesn't exist, return the index where it would be inserted
- Edge cases: target smaller than all elements, target larger than all elements, target at beginning/end

### M —> Match
This is a binary search problem:
- The array is sorted, which allows binary search
- We need to find the position or where it would go
- Standard binary search with slight modification for insertion position
- Pattern: Binary search for position finding

### P - Plan
1. Initialize left and right pointers
2. While left <= right:
   - Calculate mid index
   - If nums[mid] equals target, return mid
   - If nums[mid] < target, target must be in right half: left = mid + 1
   - If nums[mid] > target, target must be in left half: right = mid - 1
3. If target not found, left is the insertion position
4. Return left

### I - Implement
```python
class Solution:
    def searchInsert(self, nums: list[int], target: int) -> int:
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return left
```

### R - Run
Example walkthrough with nums = [1, 3, 5, 6], target = 5:
- left=0, right=3, mid=1, nums[1]=3 < 5, left=2
- left=2, right=3, mid=2, nums[2]=5 == 5, return 2

Example with target = 2:
- left=0, right=3, mid=1, nums[1]=3 > 2, right=0
- left=0, right=0, mid=0, nums[0]=1 < 2, left=1
- left=1 > right=0, exit loop, return 1

Example with target = 7:
- left=0, right=3, mid=1, nums[1]=3 < 7, left=2
- left=2, right=3, mid=2, nums[2]=5 < 7, left=3
- left=3, right=3, mid=3, nums[3]=6 < 7, left=4
- left=4 > right=3, exit loop, return 4

### E - Evaluate
Time Complexity: O(log n) where n is the length of the array
- Each iteration eliminates half of the remaining elements
- Number of iterations: log₂(n)
- Total: O(log n)

Space Complexity: O(1)
- Only using a few variables (left, right, mid)
- No extra data structures needed

Key Insight
The key insight is that when the binary search completes without finding the target, the left pointer will be at the correct insertion position. This is because left always points to the first position where the target could be inserted while maintaining sorted order. The standard binary search logic naturally leads us to this position.

What I Learned
Binary search can be adapted for finding insertion positions, not just exact matches. The pattern of using the final state of pointers (left or right) to determine the result is common in search problems. This approach is more efficient than linear search for finding insertion positions in sorted arrays.