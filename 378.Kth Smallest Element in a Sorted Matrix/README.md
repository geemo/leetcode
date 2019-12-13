Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example:**
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

**Note:**

You may assume k is always valid, 1 ≤ k ≤ n2.

**Solution:**

```golang
func kthSmallest(matrix [][]int, k int) int {
  row, col := len(matrix), len(matrix[0])
  left, right := matrix[0][0], matrix[row - 1][col - 1]

  for left < right {
    mid := left + (right - left) / 2
    count := findLessThanMidCount(matrix, mid, row, col)

    if count < k {
      left = mid + 1
    } else {
      right = mid
    }
  }

  return left
}

func findLessThanMidCount(matrix [][]int, mid, row, col int) int {
  i, j, count := row - 1, 0, 0
  for i >= 0 && j < col {
    if matrix[i][j] <= mid {
      count += i + 1
      j++
    } else {
      i--
    }
  }

  return count
}
```
