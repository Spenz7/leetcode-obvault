---
tags:
  - leetcode/solution
question: "[[19.remove-nth-node-from-end-of-list|Remove Nth Node From End of List]]"
desc:
program_language: Python
time_complexity: O(n) - regardless of 2-pass or 1-pass sln
space_complexity: O(1) - regardless of 2-pass or 1-pass sln
grade: ⭐⭐
relative_links:
cssclasses:
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
updated:
---
# Key Insight  
  - for 2-pass sln,
	  - acc for edge case where uw to remove the head node, aka if n == size
	  - when finding the node right b4 the node uw to remove, it's size-n-1 istd of size-n, rmb trace out
- 1-pass sln use fast/slow ptr technique aka 2-ptr technique for LL 
+ 1-pass sln also use dummy node to acc for case if we wan to remove the head/first node in the LL
# Non opt (2-pass sln)
```python
#tc O(n) cuz u do 2 passes of O(n) each, space O(1)
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: Optional[ListNode]
        :type n: int
        :rtype: Optional[ListNode]
        """

        #count size of LL
        cur = head
        size = 0
        while cur:
            size += 1
            cur = cur.next
        
        #edge case where if removing the head node
        if n == size:
            return head.next
        
        #if not removing the head node, get ready to move to the node right b4 the node we wan to remove
        cur = head #reset cur back to head
        for _ in range(size-n-1):
            cur = cur.next

        cur.next = cur.next.next

        return head
```

# Optimal Approach  (1-pass sln w dummy node, preferred)
nc sln (I slightly modified it but logic the same) - https://www.youtube.com/watch?v=XVuQxVej6y8
  ```python
#opt sln, see neetcode, tc O(n), sc O(1)
class Solution(object):
    def removeNthFromEnd(self, head, n):

        #y need dummy node? Cuz need acc for case where we wan to remove the first/head node
        dummy = ListNode(0, head)

        slow = dummy
        fast = head

        #we wan to ensure there r n nodes btwn slow and fast, reason is cuz later when we do the while loop, we can then mantain that gap until fast reaches None which causes slow to land right before the node to delete.
        for _ in range(n):
            fast = fast.next

        while fast:
            fast = fast.next
            slow = slow.next
        
        #by end of loop, slow shd pt to the node b4 the node we wan to del
        slow.next = slow.next.next

        return dummy.next #rmb dummy.next pts to the head
  ```
# 1-pass wo dummy node
```python
#1-pass wo dummy node
class Solution(object):
    def removeNthFromEnd(self, head, n):
        slow = head
        fast = head

        for _ in range(n):
            fast = fast.next

        # removing the head
        if fast is None:
            return head.next

        while fast.next:
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next

        return head
```
# Mistakes