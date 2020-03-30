Given an N x N grid containing only values 0 and 1, where 0 represents water and 1 represents land, find a water cell such that its distance to the nearest land cell is maximized and return the distance.

The distance used in this problem is the Manhattan distance: the distance between two cells (x0, y0) and (x1, y1) is |x0 - x1| + |y0 - y1|.

If no land or water exists in the grid, return -1.

**Example 1:**
```
Input: [[1,0,1],[0,0,0],[1,0,1]]
Output: 2
Explanation: 
The cell (1, 1) is as far as possible from all the land with distance 2.
```
**Example 2:**
```
Input: [[1,0,0],[0,0,0],[0,0,0]]
Output: 4
Explanation: 
The cell (2, 2) is as far as possible from all the land with distance 4.
```

**Note:**

- 1 <= grid.length == grid[0].length <= 100
- grid[i][j] is 0 or 1

**Solution:**

```golang
func maxDistance(grid [][]int) int {
  n := len(grid)
  var queue [][2]int
  for i := 0; i < n; i++ {
    for j := 0; j < n; j++ {
      if grid[i][j] == 1 {
        queue = append(queue, [2]int{i, j})
      }
    }
  }

  dirs := [4][2]int{[2]int{-1, 0}, [2]int{1, 0}, [2]int{0, -1}, [2]int{0, 1}}
  var step int
  for len(queue) != 0 {
    for size := len(queue); size > 0; size-- {
      r, c := queue[0][0], queue[0][1]
      queue = queue[1:]

      for _, d := range dirs {
        nr, nc := r + d[0], c + d[1]
        if nr < 0 || nr >= n || nc < 0 || nc >= n || grid[nr][nc] != 0 {
          continue
        }
        grid[nr][nc] = 1
        queue = append(queue, [2]int{nr, nc})
      }
    }

    if len(queue) != 0 {
      step++
    }
  }

  if step == 0 {
    return -1
  }
  return step
}
```
