---
tags:
  - leetcode/solution
question: "[[207.course-schedule|Course Schedule]]"
desc:
program_language: Python
time_complexity: "O(V+E), V = #courses, E = len(prerequisites)"
space_complexity: O(V+E)
grade: ⭐
relative_links:
cssclasses:
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
updated:
---
# Key Insight  
-  Store graph as adj list istd and use toposort
- Given [1,0] which means course 1 has prereq course 0.
	- Istd of storing the graph as prereq -> course, store it as course -> prereq for the DFS sln cuz it makes it easier to acc the prereq later via graph[course], cuz if u store it the other way round it's more mafan to obtain the prereqs for a given course
- Cycle detection:  
- visiting = nodes currently in DFS path  
- visited = nodes already proven to have no cycle  
  
If DFS reaches a node already in visiting:  
- cycle exists  
- return False

Other optimal approach is Kahn's Algorithm (BFS topological sort), which is also O(V + E).
# Brute Force  

# Optimal Approach  
```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        #if prerequisites is empty yet numCourses>=1, still possible irl cuz it just means those courses dun have any prereq. This case is alr implicitly handled in the code below
        
        #idx of ele in graph = courseNo, eg idx 0 is for course 0
        #graph[course] = list of prereq for that course
        graph = [[] for _ in range(numCourses)]

        for course, prereq in prerequisites:
            graph[course].append(prereq)
        
        #courses currently in the DFS path
        visiting = set()

        #courses already verified to not lead to a cycle
        visited = set()

        def dfs(course):
            #if we revisit a course in the current DFS path, we found a cycle
            if course in visiting:
                return False
            
            #alr checked b4 and confirmed safe
            if course in visited:
                return True
            
            #if cur course doesn't cause a cycle and hasn't been visited b4,
            #then start exploring this course
            visiting.add(course)

            #ensure all prereq of this course are acyclic
            for prereq in graph[course]:
                if not dfs(prereq):
                    return False

            #if all prereq of this course r acyclic, oso means u alr explore finish all prereq of this course, then u no longer need to check/visit this course, so remove it from visiting
            visiting.remove(course)

            #mark as visited/safe so we dunnid check it again
            visited.add(course)

            return True

        #If every course can be DFSed without finding a cycle, then no cycle exists anywhere in the graph.

        #DFS(course) only guarantees checking courses reachable from course.
        # Since the graph may be disconnected, we run DFS from every course to ensure every part of the graph is checked for cycles.
        # eg Course 0 may have no path to Course 5.

        for course in range(numCourses):
            if not dfs(course):
                return False
        
        return True
```
# Mistakes