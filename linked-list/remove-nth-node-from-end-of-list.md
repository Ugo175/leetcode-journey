Problem: Remove Nth Node From End of List

Difficulty: Medium

### U —> Understand
We need to remove the nth node from the end of a linked list:
- Input: head of linked list and integer n (1-indexed from the end)
- Output: head of the modified linked list
- We need to remove the nth node from the end, not the beginning
- The list is guaranteed to have at least n nodes
- Edge cases: removing head node, removing last node, single node list

### M —> Match
This is a two-pointer (fast and slow) problem with a dummy node:
- We use two pointers to create a gap of n+1 between them
- When fast reaches the end, slow will be at the node before the one to remove
- We use a dummy node to handle edge cases like removing the head
- Pattern: Two-pointer technique with fixed gap for position-based operations

### P - Plan
1. Create a dummy node pointing to head (handles edge cases)
2. Initialize both fast and slow pointers to dummy
3. Move fast pointer n+1 steps ahead to create the gap
4. Move both pointers until fast reaches the end
5. Slow will now be at the node before the one to remove
6. Remove the target node by pointing slow.next to slow.next.next
7. Return dummy.next (new head)

### I - Implement
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head

        fast, slow = dummy, dummy

        for i in range(n + 1):
            fast = fast.next

        while fast:
            slow, fast = slow.next, fast.next

        slow.next = slow.next.next

        return dummy.next
```

### R - Run
Example with list: 1 → 2 → 3 → 4 → 5, n = 2
- dummy → 1 → 2 → 3 → 4 → 5
- fast, slow = dummy
- Move fast 3 steps: fast = 3
- Move both until fast is None:
  - slow=1, fast=4
  - slow=2, fast=5
  - slow=3, fast=None
- slow is at 3, slow.next (4) should be removed
- slow.next = slow.next.next → 3 → 5
- Return dummy.next = 1 → 2 → 3 → 5

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the list
- Moving fast pointer n+1 steps: O(n)
- Moving both pointers to end: O(n)
- Total: O(n)

Space Complexity: O(1)
- Only using constant extra space (dummy node, two pointers)
- No extra data structures needed

Key Insight
The key insight is that by creating a gap of n+1 between fast and slow pointers, when fast reaches the end, slow will be exactly at the node before the one we want to remove. The dummy node is crucial because it handles the edge case where we need to remove the head node - without it, we wouldn't have a node pointing to the head.

What I Learned
The two-pointer technique with a fixed gap is powerful for position-based operations in linked lists. Using a dummy node simplifies edge cases where we might need to modify the head. This approach is more efficient than first counting the length and then traversing again (two passes) as it accomplishes the task in a single pass.