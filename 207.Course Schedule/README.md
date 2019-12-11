There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

**Example 1:**
```
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```
**Example 2:**
```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```
**Note:**

- The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
- You may assume that there are no duplicate edges in the input prerequisites.

**Solution:**

```golang
func canFinish(numCourses int, prerequisites [][]int) bool {
  if len(prerequisites) == 0 {
    return true
  }

  inDegree := make([]int, numCourses)
  for _, p := range prerequisites {
    inDegree[p[0]]++
  }

  var queue []int
  for k, v := range inDegree {
    if v == 0 {
      queue = append(queue, k)
    }
  }

  var res []int
  for len(queue) != 0 {
    num := queue[0]
    queue = queue[1:]
    res = append(res, num)

    for _, p := range prerequisites {
      if num == p[1] {
        inDegree[p[0]]--
        if inDegree[p[0]] == 0 {
          queue = append(queue, p[0])
        }
      }
    }
  }

  return len(res) == numCourses
}
```
