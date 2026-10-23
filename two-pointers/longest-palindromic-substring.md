Problem: Longest Palindromic Substring

Difficulty: Medium

### U —> Understand
We need to find the longest palindromic substring in a given string:
- Input: a string
- Output: the longest palindromic substring
- A palindrome reads the same forwards and backwards
- If there are multiple palindromes of the same maximum length, return any one
- Edge cases: empty string, single character, all same characters, no palindrome longer than 1

### M —> Match
This is a string manipulation problem with expand from center technique:
- Brute force would check all substrings: O(n³)
- Optimal approach uses the fact that palindromes expand from their center
- Each character (and gap between characters) can be a center
- Pattern: Expand from center for palindrome problems

### P - Plan
**Expand from Center Approach:**
1. Initialize longest as empty string
2. Define expand function that takes left and right pointers:
   - Expand outward while characters match and within bounds
   - Return the palindrome substring
3. For each character in the string:
   - Treat it as center of odd-length palindrome: expand(i, i)
   - Treat gap between i and i+1 as center of even-length palindrome: expand(i, i+1)
   - Update longest if either expansion yields longer palindrome
4. Return longest palindrome found

### I - Implement
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        longest = ""

        def expandFromCenter(left, right):
            while (left >= 0 and right < len(s) and s[left] == s[right]):
                left -= 1
                right += 1

            return s[left + 1 : right]

        for i in range(len(s)):
            odd = expandFromCenter(i, i)
            even = expandFromCenter(i, i + 1)

            if len(odd) > len(longest):
                longest = odd

            if len(even) > len(longest):
                longest = even

        return longest
```

### R - Run
Example walkthrough with s = "babad":
- i=0: odd expand(0,0) = "b", even expand(0,1) = "" (no match)
- i=1: odd expand(1,1) = "a", even expand(1,2) = "bab" (b==b), longest = "bab"
- i=2: odd expand(2,2) = "b", even expand(2,3) = "aba" (a==a), longest = "aba"
- i=3: odd expand(3,3) = "a", even expand(3,4) = "" (no match)
- i=4: odd expand(4,4) = "d", even expand(4,5) = "" (out of bounds)
- Return "aba" or "bab" (both are valid longest palindromes)

### E - Evaluate
Time Complexity: O(n²) where n is the length of the string
- For each of n characters, we potentially expand O(n) times
- Total: O(n²)

Space Complexity: O(1)
- Only using a few variables and the expand function uses O(1) extra space
- The returned substring is part of the original string (slicing creates new string but it's the output)
- Total: O(1)

Key Insight
The key insight is that palindromes expand symmetrically from their center. By treating each character (and each gap between characters) as a potential center, we can find all palindromes efficiently. This transforms the problem from checking all substrings (O(n³)) to expanding from centers (O(n²)).

What I Learned
The expand from center technique is powerful for palindrome problems. We need to handle both odd-length (center is a character) and even-length (center is between characters) palindromes. This approach is much more efficient than brute force and is a standard pattern for palindrome-related problems.