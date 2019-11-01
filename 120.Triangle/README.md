Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

**Note:**

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

**Solution:**

```golang
func minimumTotal(triangle [][]int) int {
    if len(triangle) == 0 || len(triangle[0]) == 0 {
        return 0
    }

    n := len(triangle)
    m := len(triangle[n - 1])
    dp := make([][]int, n)
    for i := 0; i < n; i++ {
        dp[i] = make([]int, m)
    }

    for i := 0; i < m; i++ {
        dp[n - 1][i] = triangle[n - 1][i]
    }

    for i := n - 2; i >= 0; i-- {
        for j := i; j >= 0; j-- {
            dp[i][j] = min(dp[i+1][j], dp[i+1][j+1]) + triangle[i][j]
        }
    }

    return dp[0][0]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```
