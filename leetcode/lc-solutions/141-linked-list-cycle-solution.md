---
tags:
  - leetcode/solution
question: "[[141.linked-list-cycle|Linked List Cycle]]"
desc:
program_language: Python
time_complexity: O(n)
space_complexity: O(1)
grade: ⭐⭐⭐
relative_links:
cssclasses:
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
updated:
---
# Key Insight  
  - The online judge uses `pos` behind the scenes to construct the linked list before calling your function. Your job is to detect whether a cycle exists by traversing the actual pointers, not by using `pos`.
  - for opt sln, use 2 ptr LL technique called fast and slow ptr, diff from LC 19 cuz here both ptrs move at diff speed
	  - slow ptr moves by 1 node/steps, fast moves by 2 steps, eventually they'll collide (be on the same node) cuz img a racetrack and the faster runner comes to a pt where it's right next to the slower runner
	  - y tc O(n)? Cuz 
# Non opt sln  
```python
#non opt sln, tc O(n), sc O(n) cuz got hashset that wc can store n nodes, where n = total #nodes in LL
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        #when you store an object like a Node in a dictionary in python we are storing the address in the dictionary/set (addr can be stored in hashset as they r hashable) That is how we can be sure that we have not seen it before using the "hashset" approach.
        visited = set() #membership check takes O(1)
		
        cur = head
        while cur:
            if cur in visited:
                return True
            visited.add(cur) #rmb set uses .add() istd of .append()
            cur = cur.next
        return False
```
# Optimal Approach  
https://youtu.be/gBTe7lFR3vc?si=dBrs5TNf8Y6aC3zF&t=216
  ```python
  class Solution(object):
    def hasCycle(self, head):
        slow, fast = head, head
        #check if fast can move 4ward 2 steps first, to do so, need check if can move 1 step first, so check if fast is None, then if fast isn't None, check if fast.next isn't None, if both aren't None means fast can move 4ward 2 steps

        #since fast always encounters nodes b4 slow (apart from when init both at head), once fast and fast.next aren't null, slow won't be null too
        #If slow were None, then fast would already have become None earlier (or at the same time).
        while fast and fast.next: 
        #while slow and fast and fast.next is redundant cuz dunnid check slow
            fast = fast.next.next
            slow = slow.next
            if slow==fast:
                return True
        return False
  ```
# Opt sln tc
Let:

```
m = number of nodes before the cycle
k = cycle length
n = total #nodes = m + k
```

No cycle:  
`fast` moves 2 nodes per loop, so it hits `None` in at most about `n/2` loops -> O(n).

Cycle:  
`slow` enters the cycle after `m` loops.

By that time, `fast` is already inside the cycle too, since it moves faster.

Inside the cycle:

```
slow moves 1
fast moves 2
fast gains 1 node per loop
```

The maximum gap inside the cycle is `k - 1`, this occurs when slow is at the first node of the loop, and fast is at the last node aka kth node of the loop, so the # nodes btwn them is k-1, and per iteration/loop, the gap btwn them closes by 1, so wc start off w max gap of k-1, so it takes O(k-1) iter to make gap = 0 nodes, which est= O(k). 
See utube link abv if unsure.

```
Total loops/iterations = m + O(k)
Since n = m + k, TC = O(n)
```

So TC is O(n).

Note: Once slow reaches the first node in the cycle, it only takes k-1 iter in the wc (where max gap btwn slow and fast when fast is currently at the 2nd node in the cycle). But if not, after slow reaches the 1st node in the cycle, it takes less than k-1 iter for both to point to the same node, which still is equiv to O(k) cuz we care abt wc for tc