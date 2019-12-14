Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

**Example 1:**
```
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
```
**Example 2:**
```
Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

**Solution:**

```golang
func longestIncreasingPath(matrix [][]int) int {
  row := len(matrix)
  if row == 0 {
    return 0
  }
  col := len(matrix[0])
  if col == 0 {
    return 0
  }

  dirs := [4][2]int{
    [2]int{1, 0},
    [2]int{-1, 0},
    [2]int{0, 1},
    [2]int{0, -1},
  }
  cache := make([][]int, row)
  for i := range cache {
    cache[i] = make([]int, col)
  }
  var dfs func(i, j int) int
  dfs = func(i, j int) int {
    if cache[i][j] != 0 {
      return cache[i][j]
    }
    for _, d := range dirs {
      tmp_i, tmp_j := i + d[0], j + d[1]
      if tmp_i >= 0 && tmp_i < row && tmp_j >= 0 && tmp_j < col && matrix[i][j] < matrix[tmp_i][tmp_j] {
        cache[i][j] = max(cache[i][j], dfs(tmp_i, tmp_j))
      }
    }
    cache[i][j]++
    return cache[i][j]
  }

  var ans int
  for i := 0; i < row; i++ {
    for j := 0; j < col; j++ {
      ans = max(ans, dfs(i, j))
    }
  }

  return ans
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
