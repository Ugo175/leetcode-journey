Problem: Maximum Nesting Depth of the Parentheses

Difficulty: Easy

### U - Understand
We need to find the maximum nesting depth of parentheses in a string:
- Input: a valid parentheses string consisting of '(' and ')' characters, possibly with other characters
- Output: the maximum depth of nested parentheses
- Depth is defined as the maximum number of nested parentheses
- Example: "(1+(2*3)+((8)/4))+1" has depth 3 because of "((8))"
- Edge cases: no parentheses, single pair, deeply nested, all characters are parentheses

### M - Match
This is a simple counting problem because:
- We just need to track the current depth as we iterate
- When we see '(', we're going deeper (increase depth)
- When we see ')', we're coming out (decrease depth)
- No need for a stack since we only need the count, not the actual structure
- Pattern: Simple counter for tracking nesting level

### P - Plan
1. Initialize max_depth and current_depth to 0
2. Iterate through each character in the string:
   - If character is '(': increment current_depth, update max_depth if current_depth is larger
   - If character is ')': decrement current_depth
   - Other characters: ignore
3. Return max_depth after processing all characters

### I - Implement
```python
class Solution:
    def maxDepth(self, s: str) -> int:
        maxDepth, currentDepth = 0, 0

        for char in s:
            if char == "(":
                currentDepth += 1
                maxDepth = max(currentDepth, maxDepth)
            elif char == ")":
                currentDepth -= 1

        return maxDepth
```

### R - Run
Example walkthrough with s = "(1+(2*3)+((8)/4))+1":
- '(': current=1, max=1
- '1': ignore
- '+': ignore
- '(': current=2, max=2
- '2': ignore
- '*': ignore
- '3': ignore
- ')': current=1
- '+': ignore
- '(': current=2, max=2
- '(': current=3, max=3
- '8': ignore
- ')': current=2
- '/': ignore
- '4': ignore
- ')': current=1
- ')': current=0
- '+': ignore
- '1': ignore
- Return 3

### E - Evaluate
Time Complexity: O(n) where n is the length of the string
- We iterate through the string once
- Each operation is O(1)
- Total: O(n)

Space Complexity: O(1)
- Only using two variables (maxDepth, currentDepth)
- No extra data structures needed

Key Insight
The key insight is that we don't need to actually track the structure of the parentheses - we just need to count how deep we are at any point. Since the string is guaranteed to be valid, we don't need to worry about mismatched parentheses. The depth increases with each '(' and decreases with each ')', and the maximum depth encountered is our answer.

What I Learned
Not all parentheses problems require a stack. When we only need to count or track depth levels without needing to match specific pairs or process content between them, simple counters are sufficient. This is a good example of choosing the simplest solution that meets the requirements rather than defaulting to more complex data structures.