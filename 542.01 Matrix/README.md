Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

**Example 1:**

```
Input:
[[0,0,0],
 [0,1,0],
 [0,0,0]]

Output:
[[0,0,0],
 [0,1,0],
 [0,0,0]]
```

**Example 2:**

```
Input:
[[0,0,0],
 [0,1,0],
 [1,1,1]]

Output:
[[0,0,0],
 [0,1,0],
 [1,2,1]]
```

**Note:**

The number of elements of the given matrix will not exceed 10,000.
There are at least one 0 in the given matrix.
The cells are adjacent in only four directions: up, down, left and right.

**Solution:**

```golang
// bfs
func updateMatrix(matrix [][]int) [][]int {
  row := len(matrix)
  if row == 0 {
    return matrix
  }
  col := len(matrix[0])

  ans := make([][]int, row)
  var queue [][2]int
  maxInt := intMax(row, col)
  for i := range ans {
    ans[i] = make([]int, col)
    for j := range ans[i] {
      if matrix[i][j] == 0 {
        queue = append(queue, [2]int{i, j})
        continue
      }
      ans[i][j] = maxInt
    }
  }

  dirs := [4][2]int{
    [2]int{-1, 0},
    [2]int{1, 0},
    [2]int{0, -1},
    [2]int{0, 1},
  }
  for len(queue) != 0 {
    or, oc := queue[0][0], queue[0][1]
    queue = queue[1:]
    for _, d := range dirs {
      nr, nc := or+d[0], oc+d[1]
      if nr >= 0 && nr < row && nc >= 0 && nc < col {
        if ans[nr][nc] > ans[or][oc]+1 {
          ans[nr][nc] = ans[or][oc]+1
          queue = append(queue, [2]int{nr, nc})
        }
      }
    }
  }

  return ans
}

func intMax(row, col int) int {
  if row > col {
    return row*2
  }
  return col*2
}
```

```golang
func updateMatrix(matrix [][]int) [][]int {
  m := len(matrix)
  if m == 0 {
    return nil
  }
  n := len(matrix[0])
  if n == 0 {
    return nil
  }

  var queue [][2]int
  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      if matrix[i][j] == 0 {
        queue = append(queue, [2]int{i, j})
      } else {
        matrix[i][j] = -1
      }
    }
  }

  dirs := [4][2]int{[2]int{-1, 0}, [2]int{1, 0}, [2]int{0, -1}, [2]int{0, 1}}
  for len(queue) != 0 {
    r, c := queue[0][0], queue[0][1]
    queue = queue[1:]

    for _, d := range dirs {
      nr, nc := r + d[0], c + d[1]
      if nr >= 0 && nr < m && nc >= 0 && nc < n && matrix[nr][nc] == -1 {
        matrix[nr][nc] = matrix[r][c] + 1
        queue = append(queue, [2]int{nr, nc})
      }
    }
  }

  return matrix
}
```
