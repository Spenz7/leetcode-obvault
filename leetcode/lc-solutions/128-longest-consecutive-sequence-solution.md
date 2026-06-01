---
tags:
  - leetcode/solution
question: "[[128.longest-consecutive-sequence|Longest Consecutive Sequence]]"
desc:
program_language: Python
time_complexity: bf O(n^3), opt O(n)
space_complexity: bf O(1), opt O(1)
grade: ⭐⭐
relative_links:
cssclasses:
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
updated:
---
# Key Insight  
- rmb acc for empty input list (sometimes u can implicitly handle it eg see opt sln)
- rmb acc for dup number
  
# Brute Force  
```python
#worst bf mtd, tc O(n^3), sc O(1)
# For every number:
# Treat it as the start of a sequence.
# Check whether num + 1 exists in the array.
# Check whether num + 2 exists in the array.
# Continue until the sequence breaks.
# Record the longest length.

class Solution(object):
    def longestConsecutive(self, nums):
        res = 0 #implicitly handles case where input list is empty
        for n in nums:
            streak = 1
            cur = n
            while cur+1 in nums:
                streak+=1
                cur+=1
            res = max(res, streak)
        return res 
```
```python
#dunnid mention this mtd in interview, tc O(n^2), sc O(1), it's related to sorting too which u did in ur improved sln, but this here ain't gd cuz u did a redundant inner for loop, see improved sln comments
# class Solution(object):
#     def longestConsecutive(self, nums):
#         """
#         :type nums: List[int]
#         :rtype: int
#         """
#         if not nums:
#             return 0

#         n = len(nums)
#         maxStreak = 1
#         nums.sort()
#         for i in range(n):
#             streak = 1
#             for j in range(i+1, n):
#                 if nums[j]==nums[j-1]+1:
#                     streak+=1
#                 elif nums[j]==nums[j-1]:
#                     continue
#                 else:
#                     break
#             maxStreak = max(maxStreak, streak)
#         return maxStreak
```
# Improved Solution
```python
# #tc O(nlogn), sc O(1)
class Solution(object):
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0

        n = len(nums)
        nums.sort()
        maxStreak = 1
        streak = 1
        #dunnid another inner for loop cuz u only need to do 1 pass, if u do another inner for loop to cover every diff starting pt/index, it's redundant cuz u can get the same ans from scanning the arr once frm idx 0
        for i in range(1,n):      
            if nums[i]==nums[i-1]+1:
                streak+=1
            elif nums[i]==nums[i-1]:
                continue
            else:
                maxStreak = max(maxStreak, streak)
                streak = 1 #RMB reset streak back to 1
                #dun break here cuz u still need check rest of list
        maxStreak = max(maxStreak, streak)
        return maxStreak
```
# Optimal Approach  
  ```python
#tc O(n), sc O(1)
class Solution(object):
    def longestConsecutive(self, nums):
        numSet = set(nums) #get rid of dups cuz u can't count dups
        longest = 0

        #use for n in numSet istd of for n in nums
        for n in numSet:
            # Draw all numbers as pts on a number line.
            # We wan to find the longest consec seq that a no. belongs to, eg
            # [1,2,3,4], not smaller suffixes like [2,3,4]

            # To do so, we first need to identify the start of that longest consec seq, which is a no. whose predecessor (num - 1)
            # doesn't exist in the set.

            # Since every sequence has exactly one start,
            # we only need to expand sequences from their starts.
            # Otherwise, we'd be recounting a seq we've already processed. (eg if u alr count 1 2 3 4, no pt counting a smaller seq like 2 3 4)

            if (n-1) not in numSet:
                length = 1 #can be length = 0 too
                while (n+length) in numSet:
                    length+=1
                longest = max(longest,length)
        return longest
  ```
# Mistakes