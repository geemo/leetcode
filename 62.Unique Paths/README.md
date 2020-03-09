A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Above is a 7 x 3 grid. How many possible unique paths are there?

**Example 1:**
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```
**Example 2:**
```
Input: m = 7, n = 3
Output: 28
```

**Constraints:**

- 1 <= m, n <= 100
- It's guaranteed that the answer will be less than or equal to 2 * 10 ^ 9.

**Solution:**

```golang
func uniquePaths(m int, n int) int {
  if m == 0 || n == 0 {
    return 0
  }

  dp := make([][]int, m)
  for i := range dp {
    dp[i] = make([]int, n)
    dp[i][0] = 1
  }

  for j := 0; j < n; j++ {
    dp[0][j] = 1
  }

  for i := 1; i < m; i++ {
    for j := 1; j < n; j++ {
      dp[i][j] = dp[i-1][j] + dp[i][j-1]
    }
  }

  return dp[m-1][n-1]
}
```
