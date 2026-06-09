---
tags:
  - leetcode/solution
question: "[[200.number-of-islands|Number of Islands]]"
desc:
program_language: Python
time_complexity: O(mn)
space_complexity: O(mn)
grade: ⭐⭐⭐
relative_links: https://www.youtube.com/watch?v=gCswsDauXPc
cssclasses:
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
updated:
---
# Key Insight  
Find a 1?

    Island count += 1

    Flood-fill the entire island using DFS

    Mark everything visited

  

Continue scanning.
Don't think of DFS as "searching for islands".

Think of DFS as:
Erase this island so I won't count it again.

# Why sc is O(mn)? Consider the worst case:

```
1 1 11 1 11 1 1
```

or more generally:

```
m x n gridall cells are land
```

When DFS starts at the first cell, it may recursively visit every cell before unwinding.

Conceptually, the call stack could look like:

```
dfs(0,0)  dfs(0,1)    dfs(0,2)      dfs(1,2)        dfs(2,2)          ...
```

In the worst case, there can be:

```
m * n
```

active **recursive calls on the stack.**

Therefore:

```
Recursion stack space = O(mn)
```

and thus:

```
TC = O(mn)SC = O(mn)
```

for recursive DFS.
  
# Brute Force  
- dc
# Optimal Approach  
  ```python
  class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        #grid[row][col]
        #qn say m>=1 and n>-300 so dunnid acc for empty grid

        m, n = len(grid), len(grid[0])
        
        def dfs(i,j):
            #recur fn so need base case/terminating cond
            if i<0 or i>=m or j<0 or j>=n or grid[i][j]!='1':
                return
            else:
                grid[i][j] = '#' #mark as visited, technically can use '0' too
                dfs(i-1,j)
                dfs(i,j+1)
                dfs(i+1,j)
                dfs(i,j-1)

        islandCount = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]=='1':
                    count+=1
                    dfs(i,j)
        return islandCount
  ```
# Mistakes
- dun increase islandCount within the dfs fn's else block, cuz that counts the no. of 1s istd per island, but if uw got 2 ways to do so:
- Counting the Number of 1s (Area) in an Island
---------------------------------------------

### Method 1: Shared Counter (global/nonlocal)

**Idea**

-   Before DFS on an island, initialize `count = 0`.
-   Every time DFS visits a land cell, do `count += 1`.
-   After DFS finishes, `count` contains the area of the current island.

```python
count = 0

def dfs(r, c):
    nonlocal count

    if invalid:
        return

    count += 1
    mark_visited

    dfs(up)
    dfs(right)
    dfs(down)
    dfs(left)
```

**Usage**

```python
count = 0
dfs(r, c)area = count
```

**Important**

-   Must reset `count` before each island.
-   If not reset, `count` becomes the total number of land cells across all islands.

* * * * *

### Method 2: Return Values from DFS (Preferred)

**Idea**

-   Each DFS call returns the area contributed by the current cell.
-   Parent calls add up the returned values.

```python
def dfs(r, c):
    if invalid:
        return 0

    grid[r][c] = "#" #can be 0 istd of # too

    return (
        1
        + dfs(up)
        + dfs(right)
        + dfs(down)
        + dfs(left)
    )
```

**Usage**

```
area = dfs(r, c)
```

**Why it Works**

-   Current cell contributes `1`.
-   Recursive calls contribute the area of connected land.
-   Sum = area of the entire island.

* * * * *

Comparison
----------

### Method 1

-   Uses shared mutable state (`count` variable).
-   Need to remember to reset `count` before each island.
-   Easier to make mistakes.

### Method 2

-   No global/nonlocal variables needed.
-   Each DFS naturally returns the area of the current island.
-   Usually preferred in interviews.

### Both Methods

Both compute the same thing:

```
Area of the current island= Number of 1s in that island
```