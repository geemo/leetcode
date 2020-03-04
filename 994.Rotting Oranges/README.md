In a given grid, each cell can have one of three values:

- the value 0 representing an empty cell;
- the value 1 representing a fresh orange;
- the value 2 representing a rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

**Example 1:**
```
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```
**Example 2:**
```
Input: [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
```
**Example 3:**
```
Input: [[0,2]]
Output: 0
Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.
```

**Note:**

- 1 <= grid.length <= 10
- 1 <= grid[0].length <= 10
- grid[i][j] is only 0, 1, or 2.

**Solution:**

```golang
func orangesRotting(grid [][]int) int {
  m, n := len(grid), len(grid[0])

  var queue [][2]int
  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      if grid[i][j] == 2 {
        queue = append(queue, [2]int{i, j})
      }
    }
  }

  var ans int
  dirs := [4][2]int{[2]int{-1, 0}, [2]int{1, 0}, [2]int{0, -1}, [2]int{0, 1}}
  for len(queue) != 0 {
    var level [][2]int
    for len(queue) != 0 {
      r, c := queue[0][0], queue[0][1]
      queue = queue[1:]
      for _, d := range dirs {
        nr, nc := r + d[0], c + d[1]
        if nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1 {
          grid[nr][nc] = 2
          level = append(level, [2]int{nr, nc})
        }
      }
    }
    queue = level
    if len(level) != 0 {
      ans++
    }
  }

  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      if grid[i][j] == 1 {
        return -1
      }
    }
  }

  return ans
}
```
