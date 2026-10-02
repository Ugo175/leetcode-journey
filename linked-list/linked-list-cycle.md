Problem: Linked List Cycle

Difficulty: Easy

### U —> Understand
We need to determine if a linked list has a cycle:
- Input: head of a linked list
- Output: True if there is a cycle, False otherwise
- A cycle means some node's next pointer points to a previous node in the list
- The list may be empty or have no cycle
- Edge cases: empty list, single node, cycle at different positions, no cycle

### M —> Match
This is a Floyd's Cycle Detection (tortoise and hare) problem:
- We use two pointers moving at different speeds
- If there's a cycle, the fast pointer will eventually catch up to the slow pointer
- If there's no cycle, the fast pointer will reach the end
- Pattern: Two-pointer technique with different speeds for cycle detection

### P — Plan
1. Handle edge case: if head is None, return False
2. Initialize both fast and slow pointers to head
3. Move fast pointer by 2 steps, slow pointer by 1 step
4. If fast or fast.next becomes None, there's no cycle
5. If fast and slow pointers ever meet, there's a cycle
6. Return False if we exit the loop without meeting

### I - Implement
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if not head:
            return False

        fast = head
        slow = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True

        return False
```

### R - Run
Example with cycle: 1 → 2 → 3 → 4 → 2 (cycle back to 2)
- fast=1, slow=1
- Iteration 1: slow=2, fast=3
- Iteration 2: slow=3, fast=2 (fast went 3→4→2)
- Iteration 3: slow=4, fast=4 (slow went 3→4, fast went 2→3→4)
- slow == fast, return True

Example without cycle: 1 → 2 → 3 → None
- fast=1, slow=1
- Iteration 1: slow=2, fast=3
- Iteration 2: slow=3, fast=None (fast went 3→None)
- fast is None, exit loop, return False

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the list
- If there's a cycle, fast pointer will catch slow in at most n steps
- If there's no cycle, fast pointer reaches end in n/2 steps
- Total: O(n)

Space Complexity: O(1)
- Only using two pointers (fast, slow)
- No extra data structures needed

Key Insight
The key insight is that if there's a cycle, the fast pointer (moving 2 steps) will eventually "lap" the slow pointer (moving 1 step) and they will meet. If there's no cycle, the fast pointer will reach the end (None) before catching the slow pointer. This elegant algorithm detects cycles without using extra memory like a visited set.

What I Learned
Floyd's Cycle Detection is a classic algorithm that's much more space-efficient than using a hash set to track visited nodes. The two-pointer technique with different speeds is a powerful pattern for cycle detection. This algorithm also has interesting mathematical properties - the meeting point can be used to find the start of the cycle.