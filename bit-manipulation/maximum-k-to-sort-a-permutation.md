Problem: Maximum K to Sort a Permutation

Difficulty: Medium

### U —> Understand
We need to find the maximum K such that we can sort the permutation:
- Input: array representing a permutation of [0, 1, 2, ..., n-1]
- Output: maximum K value for sorting
- The operation involves checking index-value mismatches
- Edge cases: already sorted, completely reversed, single element

### M —> Match
This is a bitwise operation problem:
- We iterate through the permutation checking index-value mismatches
- Use bitwise AND to find common properties of mismatched elements
- Pattern: Iteration with bitwise operations for property finding

### P - Plan
1. Initialize answer to -1 (bitwise AND identity)
2. Iterate through each index and value pair:
   - If index doesn't equal value (element is out of place)
   - Perform bitwise AND with the value
3. Return max of result and 0 (to handle negative cases)

### I - Implement
```python
class Solution:
    def sortPermutation(self, nums: List[int]) -> int:
        ans = -1

        for i, x in enumerate(nums):
            if i != x:
                ans &= x

        return max(ans, 0)
```

### R - Run
Example walkthrough with nums = [0, 2, 1]:
- i=0, x=0: i == x, skip
- i=1, x=2: i != x, ans = -1 & 2 = -2
- i=2, x=1: i != x, ans = -2 & 1 = -2
- Return max(-2, 0) = 0

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- Single pass through the array
- Each operation is O(1)
- Total: O(n)

Space Complexity: O(1)
- Only using a few variables
- No extra data structures needed
- Total: O(1)

Key Insight
The key insight is using bitwise AND operation to find common properties among mismatched elements. Starting with -1 (all bits set in two's complement) and AND-ing with mismatched values helps identify shared characteristics that might be relevant for determining the maximum K.

What I Learned
Bitwise operations can be used to find common properties across multiple values efficiently. The pattern of starting with an identity value for the operation and accumulating results through iteration is common. This approach is useful when we need to find shared characteristics or constraints across elements.