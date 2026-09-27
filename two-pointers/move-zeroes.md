Problem: Move Zeroes

Difficulty: Easy

### U - Understand
We need to move all zeroes to the end of the array while maintaining the relative order of non-zero elements:
- Input: array of integers
- Output: modify the array in-place (no return value)
- All zeroes must be moved to the end
- Non-zero elements must maintain their relative order
- Must minimize total operations
- Edge cases: all zeroes, no zeroes, single element, all non-zeroes

### M - Match
This is a two-pointer problem because:
- We need to move elements while maintaining order
- One pointer can track the position for the next non-zero element
- Another pointer scans through the array
- Pattern: Two-pointer technique for in-place array modification

### P - Plan
1. Initialize two pointers: left (for placing non-zeroes) and right (for scanning)
2. While right pointer hasn't reached the end:
   - If current element is non-zero:
     - Swap elements at left and right positions
     - Move both pointers forward
   - If current element is zero:
     - Only move right pointer forward
3. This ensures non-zeroes are moved to the front in their original order, zeroes naturally end up at the end

### I - Implement
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        left, right = 0, 0
        nums_length = len(nums)

        while right < nums_length:
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right += 1
            else:
                right += 1
```

### R - Run
Example walkthrough with nums = [0, 1, 0, 3, 12]:
- left=0, right=0: nums[0]=0, right=1
- left=0, right=1: nums[1]=1≠0, swap nums[0]↔nums[1]: [1,0,0,3,12], left=1, right=2
- left=1, right=2: nums[2]=0, right=3
- left=1, right=3: nums[3]=3≠0, swap nums[1]↔nums[3]: [1,3,0,0,12], left=2, right=4
- left=2, right=4: nums[4]=12≠0, swap nums[2]↔nums[4]: [1,3,12,0,0], left=3, right=5
- Final: [1, 3, 12, 0, 0]

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- We iterate through the array once with the right pointer
- Each swap operation is O(1)
- Total operations: O(n)

Space Complexity: O(1)
- Only using a few variables (left, right, nums_length)
- No extra data structures needed
- In-place modification

Key Insight
The key insight is that we don't need to actually "move" zeroes - we just need to ensure non-zeroes are placed in their correct positions. By using two pointers, one for where the next non-zero should go and one for scanning, we can maintain order while minimizing operations. When left and right point to the same position (both at a non-zero), the swap is essentially a no-op.

What I Learned
Two-pointer techniques are powerful for in-place array modifications. The pattern of "one pointer for placement, one for scanning" is common for problems where we need to reorder elements while maintaining certain properties. This approach is more efficient than creating a new array or multiple passes.