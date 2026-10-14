Problem: Next Greater Element I

Difficulty: Easy

### U —> Understand
We need to find the next greater element for each element in nums1:
- Input: two arrays nums1 (subset) and nums2 (superset)
- Output: array where each element is the next greater element in nums2 for the corresponding element in nums1
- The next greater element is the first element to the right that is greater
- If no greater element exists, output -1
- nums1 is a subset of nums2
- Edge cases: no greater elements, all elements have greater elements, duplicates

### M —> Match
This is a monotonic stack problem:
- We can use a monotonic decreasing stack to find next greater elements
- Process nums2 once to build a mapping of each element to its next greater element
- Then use this mapping to answer queries for nums1
- Pattern: Monotonic stack for next greater element problems

### P - Plan
1. Initialize empty stack and dictionary for next greater mapping
2. Iterate through nums2:
   - While stack is not empty and current element > stack's top:
     - Pop from stack and map it to current element (its next greater)
   - Push current element onto stack
3. For remaining elements in stack (no greater element), map them to -1
4. Build result array by looking up each element from nums1 in the mapping
5. Return result array

### I - Implement
```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        next_greater = {}

        for num in nums2:
            while stack and num > stack[-1]:
                next_greater[stack.pop()] = num
            stack.append(num)

        while stack:
            next_greater[stack.pop()] = -1

        return [next_greater[num] for num in nums1]
```

### R - Run
Example walkthrough with nums1 = [4,1,2], nums2 = [1,3,4,2]:
- Process nums2:
  - num=1: stack=[1]
  - num=3: 3>1, pop 1, next_greater[1]=3, stack=[3]
  - num=4: 4>3, pop 3, next_greater[3]=4, stack=[4]
  - num=2: 2<4, stack=[4,2]
- Remaining stack: pop 2, next_greater[2]=-1; pop 4, next_greater[4]=-1
- next_greater = {1:3, 3:4, 4:-1, 2:-1}
- Build result for nums1:
  - nums1[0]=4: next_greater[4]=-1
  - nums1[1]=1: next_greater[1]=3
  - nums1[2]=2: next_greater[2]=-1
- Return [-1,3,-1]

### E - Evaluate
Time Complexity: O(n + m) where n = len(nums1), m = len(nums2)
- Processing nums2: each element pushed and popped at most once: O(m)
- Building result array: look up each element in nums1: O(n)
- Total: O(n + m)

Space Complexity: O(m)
- Stack can contain up to m elements in worst case
- Dictionary stores up to m entries
- Total: O(m)

Key Insight
The key insight is that a monotonic decreasing stack naturally helps us find the next greater element. When we encounter a number that's greater than the stack's top, we know it's the next greater element for everything on the stack that's smaller. This allows us to process the array once and build the complete mapping.

What I Learned
Monotonic stacks are powerful for "next greater/smaller element" problems. The pattern of maintaining a decreasing stack and resolving elements when the order is violated is elegant and efficient. This approach converts what could be an O(n×m) nested loop problem into O(n + m) by preprocessing the larger array once.