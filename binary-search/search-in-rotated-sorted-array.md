Problem: Search in Rotated Sorted Array

Difficulty: Medium

### U - Understand
We need to search for a target value in a rotated sorted array:
- Input: array sorted in ascending order, then rotated at an unknown pivot, and a target integer
- Output: index of target if found, -1 if not found
- The array has no duplicates
- Example: [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2] if rotated at pivot 4
- Edge cases: array not rotated, target at pivot point, target not present

### M - Match
This is a modified binary search problem because:
- The array is still mostly sorted, just rotated
- One half of the array (left or right of mid) is always sorted
- We can determine which half is sorted and check if target lies in that half
- Pattern: Binary search with additional logic to handle rotation

### P - Plan
1. Initialize left and right pointers
2. While left <= right:
   - Calculate mid index
   - If nums[mid] equals target, return mid
   - Determine which half is sorted:
     - If nums[left] <= nums[mid], left half is sorted
       - Check if target lies in sorted left half
       - If yes, search left half; otherwise, search right half
     - Else, right half is sorted
       - Check if target lies in sorted right half
       - If yes, search right half; otherwise, search left half
3. If loop completes without finding target, return -1

### I - Implement
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # Left half is sorted
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # Right half is sorted
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

### R - Run
Example walkthrough with nums = [4,5,6,7,0,1,2], target = 0:
- left=0, right=6, mid=3, nums[3]=7, left half [4,5,6,7] is sorted
- target=0 not in [4,7), left=4
- left=4, right=6, mid=5, nums[5]=1, left half [0,1] is sorted
- target=0 in [0,1), right=4
- left=4, right=4, mid=4, nums[4]=0 == target, return 4

Example with target = 3:
- Similar process, but target not found, return -1

### E - Evaluate
Time Complexity: O(log n) where n is the length of the array
- Each iteration eliminates half of the remaining elements
- Number of iterations: log₂(n)
- Same as standard binary search

Space Complexity: O(1)
- Only using a few variables (left, right, mid)
- No extra data structures needed

Key Insight
The key insight is that in a rotated sorted array, at least one half (left or right of mid) is always sorted. By identifying which half is sorted, we can determine if the target lies in that half using simple range checks. This allows us to maintain the O(log n) complexity of binary search even with the rotation.

What I Learned
Binary search can be adapted to handle modified sorted arrays. The key is to identify properties that are still true despite the modification (one half being sorted). This pattern of "identify which part is still ordered" extends to other problems with partially ordered data structures.