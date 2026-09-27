Problem: Reverse Substrings Between Each Pair of Parentheses

Difficulty: Medium

### U - Understand
We need to reverse the strings in each pair of matching parentheses, starting from the innermost:
- Input: a string consisting of lowercase English letters and parentheses
- Output: the string after reversing all substrings in parentheses
- We process from innermost to outermost parentheses
- All parentheses are guaranteed to be well-formed
- Edge cases: no parentheses, deeply nested parentheses, adjacent parentheses

### M - Match
This is a stack problem because:
- We need to handle nested structures (parentheses)
- When we encounter a closing parenthesis, we need to reverse everything since the matching opening parenthesis
- Stack naturally handles the nested/recursive nature of parentheses
- Pattern: Stack for processing nested structures with reversal

### P - Plan
1. Initialize a stack to build the result
2. Iterate through each character in the string:
   - If character is not a closing parenthesis, push it onto the stack
   - If character is a closing parenthesis:
     - Pop characters from stack into a temporary list until we find the opening parenthesis
     - Pop the opening parenthesis (don't include it in temp)
     - Extend the stack with the reversed temporary list
3. After processing all characters, join the stack to form the final string

### I - Implement
```python
class Solution:
    def reverseParentheses(self, s: str) -> str:
        stack = []

        for char in s:
            if char == ")":
                temp = []

                while stack[-1] != "(":
                    temp.append(stack.pop())

                stack.pop()  # Remove the opening parenthesis

                stack.extend(temp)
            else:
                stack.append(char)

        return "".join(stack)
```

### R - Run
Example walkthrough with s = "(abcd)":
- '(': stack=['(']
- 'a': stack=['(', 'a']
- 'b': stack=['(', 'a', 'b']
- 'c': stack=['(', 'a', 'b', 'c']
- 'd': stack=['(', 'a', 'b', 'c', 'd']
- ')': pop until '(', temp=['d','c','b','a'], pop '(', extend: stack=['d','c','b','a']
- Return "dcba"

Example with s = "(u(love)i)":
- Process: '(' push, 'u' push, '(' push, 'l','o','v','e' push
- First ')': temp=['e','v','o','l'], pop '(', extend: stack=['(','u','e','v','o','l']
- 'i' push: stack=['(','u','e','v','o','l','i']
- Second ')': temp=['i','l','o','v','e','u'], pop '(', extend: stack=['i','l','o','v','e','u']
- Return "iloveu"

### E - Evaluate
Time Complexity: O(n²) in the worst case
- n = length of the string
- Each character is pushed once: O(n)
- In worst case (deeply nested), we might pop and push the same characters multiple times
- For a string like "(((...)))", each character could be processed O(n) times
- Total: O(n²)

Space Complexity: O(n)
- The stack stores at most n characters
- The temporary list stores at most n characters in the worst case
- Total: O(n)

Key Insight
The key insight is that when we encounter a closing parenthesis, we've just completed a pair and need to reverse everything inside. By using a stack, we naturally handle the nested structure - the most recent opening parenthesis is at the top of the stack, and everything after it (until now) belongs inside that pair. The reversal happens because we pop into a temporary list (which reverses order) and then extend back onto the stack.

What I Learned
Stacks are ideal for processing nested structures like parentheses. The pattern of "process when you encounter the closing marker" is powerful. This problem shows how stack operations can be used to simulate recursive processing without actual recursion. The time complexity consideration is important - nested structures can lead to repeated processing of the same elements.