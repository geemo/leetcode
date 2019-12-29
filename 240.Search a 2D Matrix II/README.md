Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example:**
```
Consider the following matrix:

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = 5, return true.

Given target = 20, return false.

**Solution:**

```golang
func searchMatrix(matrix [][]int, target int) bool {
  m := len(matrix)
  if m == 0 {
    return false
  }
  n := len(matrix[0])
  if n == 0 {
    return false
  }

  r, c := m - 1, 0
  for r >= 0 && c < n {
    if matrix[r][c] > target {
      r--
    } else if matrix[r][c] < target {
      c++
    } else {
      return true
    }
  }

  return false
}
```
