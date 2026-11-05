Problem: Maximum Sum of Squares

Difficulty: Medium

### U —> Understand
We need to find the maximum number with given digit count and digit sum:
- Input: num (number of digits), sum (sum of digits)
- Output: string representing the maximum number that satisfies constraints
- The number must have exactly `num` digits
- The sum of digits must equal `sum`
- We want the maximum possible number (lexicographically largest)
- Edge cases: impossible sum (sum > 9*num), sum = 0, num = 1

### M —> Match
This is a greedy problem:
- To maximize the number, we want the largest digits in the most significant positions
- Greedy strategy: use as many 9s as possible (largest digit), then the remainder, then zeros
- Pattern: Greedy construction with digit placement

### P - Plan
1. Check if it's impossible: if sum > 9 * num, return empty string
2. Calculate maximum number of 9s: nines = sum // 9
3. Calculate remainder: rem = sum % 9
4. Construct digits array:
   - Add nines copies of "9"
   - If remainder exists, add it as a digit
   - Fill remaining positions with "0" (smallest digit)
5. Join and return the result

### I - Implement
```python
class Solution:
    def maxSumOfSquares(self, num: int, sum: int) -> str:
        if sum > 9 * num:
            return ""

        nines = sum // 9
        rem = sum % 9

        digits = ["9"] * nines

        if rem:
            digits.append(str(rem))

        digits.extend(["0"] * (num - len(digits)))

        return "".join(digits)
```

### R - Run
Example walkthrough with num = 5, sum = 25:
- Check: 25 > 9*5 = 45? No, proceed
- nines = 25 // 9 = 2
- rem = 25 % 9 = 7
- digits = ["9", "9", "7"]
- Remaining positions: 5 - 3 = 2
- digits = ["9", "9", "7", "0", "0"]
- Return "99700"

Example with num = 3, sum = 0:
- Check: 0 > 9*3 = 27? No, proceed
- nines = 0 // 9 = 0
- rem = 0 % 9 = 0
- digits = []
- Remaining positions: 3 - 0 = 3
- digits = ["0", "0", "0"]
- Return "000"

Example with num = 2, sum = 20:
- Check: 20 > 9*2 = 18? Yes, return ""

### E - Evaluate
Time Complexity: O(num)
- Creating array of size num
- String concatenation at the end
- Total: O(num)

Space Complexity: O(num)
- Storing digits array of size num
- Total: O(num)

Key Insight
The key insight is that to maximize the number, we want the largest possible digits in the most significant positions. 9 is the largest digit, so we use as many 9s as possible. The remainder digit fills the remaining sum requirement. Zeros fill the remaining digit count requirement while being the smallest possible digit, ensuring the most significant positions have the largest values.

What I Learned
Greedy algorithms are powerful for construction problems where we want to maximize or minimize under constraints. The pattern of "use the largest possible option first, then handle the remainder, then fill with smallest" is common. Understanding the greedy choice property (that local optimal choices lead to global optimum) is crucial for applying this approach correctly.