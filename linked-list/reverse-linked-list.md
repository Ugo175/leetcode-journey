Problem: Reverse Linked List

Difficulty: Easy

### U —> Understand
We need to reverse a singly linked list:
- Input: head of a singly linked list
- Output: head of the reversed linked list
- We need to reverse the direction of all pointers
- The list may be empty or have only one node
- Edge cases: empty list, single node, two nodes, long list

### M —> Match
This is a linked list pointer manipulation problem:
- We need to change the next pointers of each node
- We need to keep track of three nodes: current, previous, and next
- We iterate through the list, reversing each pointer as we go
- Pattern: Three-pointer technique for linked list reversal

### P — Plan
1. Initialize prev to None and current to head
2. While current is not None:
   - Save the next node (current.next) before we change the pointer
   - Reverse the pointer: set current.next to prev
   - Move prev forward: set prev to current
   - Move current forward: set current to the saved next node
3. When current becomes None, prev is at the new head
4. Return prev

### I - Implement
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev, current = None, head

        while current:
            next_node = current.next
            current.next = prev

            prev = current
            current = next_node

        return prev
```

### R - Run
Example walkthrough with list: 1 → 2 → 3 → None
- prev=None, current=1→2→3→None
- Iteration 1: next_node=2→3→None, 1→None, prev=1→None, current=2→3→None
- Iteration 2: next_node=3→None, 2→1→None, prev=2→1→None, current=3→None
- Iteration 3: next_node=None, 3→2→1→None, prev=3→2→1→None, current=None
- Return 3→2→1→None

### E - Evaluate
Time Complexity: O(n) where n is the number of nodes in the list
- We traverse the list exactly once
- Each operation within the loop is O(1)
- Total: O(n)

Space Complexity: O(1)
- Only using three pointers (prev, current, next_node)
- No extra data structures needed
- In-place reversal

Key Insight
The key insight is that we need to save the next node before we change the current node's pointer. If we change current.next to prev without saving the original next, we lose access to the rest of the list. The three-pointer pattern (current, prev, next) allows us to safely reverse each pointer while maintaining our position in the list.

What I Learned
Linked list manipulation often requires careful pointer management. The pattern of "save next, change pointer, advance pointers" is fundamental for many linked list operations. This in-place reversal is efficient and doesn't require additional memory allocation, making it preferable to creating a new list.