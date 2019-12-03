Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

**Example 1:**

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

**Solution:**

```golang
func spiralOrder(matrix [][]int) []int {
  m := len(matrix)
  if m == 0 {
    return nil
  }
  n := len(matrix[0])
  if n == 0 {
    return nil
  }

  u, d, l, r := 0, m - 1, 0, n - 1

  var ans []int
  for true {
    for i := l; i <= r; i++ {
      ans = append(ans, matrix[u][i])
    }
    u++
    if u > d {
      break
    }
    for i := u; i <= d; i++ {
      ans = append(ans, matrix[i][r])
    }
    r--
    if r < l {
      break
    }
    for i := r; i >= l; i-- {
      ans = append(ans, matrix[d][i])
    }
    d--
    if d < u {
      break
    }
    for i := d; i >= u; i-- {
      ans = append(ans, matrix[i][l])
    }
    l++
    if l > r {
      break
    }
  }

  return ans
}
```
