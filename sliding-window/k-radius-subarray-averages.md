Problem: K Radius Subarray Averages

Difficulty: Medium

### U - Understand
We need to calculate the k-radius average for each element in the array:
- Input: array of integers and integer k (radius)
- Output: array where each element at index i is the average of elements from i-k to i+k (inclusive)
- If there aren't enough elements for the k-radius (i.e., i-k < 0 or i+k >= n), output -1 for that position
- When k = 0, the average is just the element itself
- Edge cases: k larger than array size, empty array, k = 0

### M - Match
This is a sliding window problem because:
- We need to calculate averages for fixed-size windows centered at each index
- Window size is 2k + 1 (k elements on each side plus the center)
- We can efficiently update the window sum by removing the leftmost element and adding the new rightmost element
- Pattern: Fixed-size sliding window for efficient computation

### P - Plan
1. Handle edge case: if k = 0, return the original array (average of single element is itself)
2. Initialize output array with all -1 values
3. Calculate window size = 2k + 1
4. If window size > array length, return all -1s (no valid windows)
5. Calculate the sum of the first window (indices 0 to 2k)
6. Set the average at index k (center of first window)
7. Slide the window from index k+1 to n-k-1:
   - Add the new element at i+k
   - Remove the element at i-k-1
   - Calculate and set the average at index i
8. Return the output array

### I - Implement
```python
class Solution:
    def getAverages(self, nums: list[int], k: int) -> list[int]:
        if k == 0:
            return nums

        n = len(nums)
        output = [-1] * n

        window_size = 2 * k + 1

        if window_size > n:
            return output

        window_sum = sum(nums[:window_size])

        output[k] = window_sum // window_size

        for i in range(k + 1, n - k):
            window_sum += nums[i + k]
            window_sum -= nums[i - k - 1]

            output[i] = window_sum // window_size

        return output
```

### R - Run
Example walkthrough with nums = [7, 4, 3, 9, 1, 8, 5, 2, 6], k = 3:
- window_size = 7, n = 9
- First window: [7, 4, 3, 9, 1, 8, 5], sum = 37, output[3] = 37//7 = 5
- i=4: add nums[7]=2, remove nums[0]=7, sum = 32, output[4] = 32//7 = 4
- i=5: add nums[8]=6, remove nums[1]=4, sum = 34, output[5] = 34//7 = 4
- Return [-1, -1, -1, 5, 4, 4, -1, -1, -1]

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- Initial sum calculation: O(window_size) = O(k)
- Sliding window: O(n - 2k) iterations
- Each slide operation is O(1)
- Total: O(n)

Space Complexity: O(n)
- Output array stores n elements
- Only a few additional variables used
- Total: O(n)

Key Insight
The key insight is that the k-radius average for consecutive indices uses overlapping windows. Instead of recalculating the sum for each window, we can slide the window by removing the element that's leaving and adding the element that's entering. This reduces the complexity from O(n×k) to O(n).

What I Learned
Sliding window is essential for problems involving fixed-size subarray computations. The pattern of "compute initial sum, then slide with remove-left/add-right" is a standard template. Edge cases like when the window is larger than the array or when k=0 need special handling.