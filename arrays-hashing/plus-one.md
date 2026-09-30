Problem: Plus One

Difficulty: Easy

### U —> Understand
We need to increment a large integer represented as an array of digits:
- Input: array of digits representing a non-negative integer (most significant digit first)
- Output: array of digits representing the integer plus one
- The array contains no leading zeros (except for the number 0 itself)
- Edge cases: all 9s (e.g., [9,9] → [1,0,0]), no carry needed, single digit

### M —> Match
This is an array manipulation problem with carry handling:
- We need to handle the addition with potential carry propagation
- Working from right to left (least significant to most significant digit)
- Similar to manual addition with carry
- Pattern: Array manipulation with carry propagation

### P — Plan
1. Start from the rightmost digit (least significant)
2. Add 1 to the current digit
3. If the digit becomes 10, set it to 0 and carry 1 to the next digit
4. If the digit is less than 10, no more carry needed, return the result
5. If we process all digits and still have a carry, prepend 1 to the array

### I - Implement
```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        # Alternative approach: string conversion (your original solution)
        digit = ''.join(str(num) for num in digits)
        new_number = int(digit) + 1
        return [int(c) for c in str(new_number)]
```

Alternative efficient approach without string conversion:
```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        n = len(digits)
        
        for i in range(n - 1, -1, -1):
            if digits[i] < 9:
                digits[i] += 1
                return digits
            digits[i] = 0
        
        # If we're here, all digits were 9
        return [1] + digits
```

### R - Run
Example walkthrough with digits = [1, 2, 3]:
- i=2: digits[2]=3 < 9, digits[2]=4, return [1, 2, 4]

Example with digits = [9, 9]:
- i=1: digits[1]=9, set to 0
- i=0: digits[0]=9, set to 0
- All digits processed, return [1] + [0, 0] = [1, 0, 0]

Example with digits = [4, 3, 2, 1]:
- i=3: digits[3]=1 < 9, digits[3]=2, return [4, 3, 2, 2]

### E - Evaluate
Time Complexity: O(n) where n is the length of the array
- String approach: join is O(n), int conversion is O(n), list comprehension is O(n)
- Efficient approach: at most n iterations in the worst case
- Total: O(n)

Space Complexity: O(n)
- String approach: creates string and new list, both O(n)
- Efficient approach: O(1) extra space (modifies in-place), O(n) for new array only when all 9s
- Total: O(n)

Key Insight
The key insight is that addition with carry propagates from right to left. When we encounter a digit less than 9, we can simply increment it and stop - no further carry is needed. Only when we have a sequence of 9s do we need to continue the carry propagation. The all-9s case is special because we need to add a new most significant digit.

What I Learned
While string conversion is a valid and readable solution, the mathematical approach is more efficient and handles the problem's constraints better. The pattern of "process from right to left with carry" is common for problems involving arithmetic on digit arrays. Understanding when to use mathematical operations versus string operations is important for optimization.