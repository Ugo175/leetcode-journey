Problem: Middle of the Linked List

Difficulty: Easy

### U —> Understand
We need to find the middle node of a linked list:
- Input: head of a singly linked list
- Output: the middle node
- If the list has an even number of nodes, return the second middle node
- The list is non-empty (at least one node)
- Edge cases: single node, two nodes, odd number of nodes, even number of nodes

### M —> Match
This is a two-pointer (fast and slow) problem:
- We use two pointers moving at different speeds
- Fast pointer moves 2 steps, slow pointer moves 1 step
- When fast reaches the end, slow will be at the middle
- Pattern: Two-pointer technique with different speeds for finding middle position

### P — Plan
1. Initialize both fast and slow pointers to head
2. While fast is not None and fast.next is not None:
   - Move slow pointer by 1 step
   - Move fast pointer by 2 steps
3. When fast reaches the end, slow will be at the middle
4. Return slow (the middle node)

### I - Implement
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast, slow = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        return slow
```

### R - Run
Example with odd number of nodes: 1 → 2 → 3 → 4 → 5 → None
- fast=1, slow=1
- Iteration 1: slow=2, fast=3
- Iteration 2: slow=3, fast=5
- fast.next is None, exit loop
- Return slow=3 (middle node)

Example with even number of nodes: 1 → 2 → 3 → 4 → None
- fast=1, slow=1
- Iteration 1: slow=2, fast=3
- Iteration 2: slow=3, fast=None (fast went 3→None)
- fast is None, exit loop
- Return slow=3 (second middle node)

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the list
- We traverse the list once
- Fast pointer visits each node once, slow pointer visits half the nodes
- Total: O(n)

Space Complexity: O(1)
- Only using two pointers (fast, slow)
- No extra data structures needed

Key Insight
The key insight is that by moving the fast pointer twice as fast as the slow pointer, when the fast pointer reaches the end, the slow pointer will be exactly at the middle. For even-length lists, the fast pointer reaches None (end), and for odd-length lists, the fast pointer reaches the last node. This elegantly handles both cases without needing to count nodes first.

What I Learned
The fast-slow pointer technique is versatile for linked list problems. It can be used for finding the middle, detecting cycles, and even finding the kth element from the end. This approach is more efficient than counting nodes first (which requires two passes) as it finds the middle in a single pass.