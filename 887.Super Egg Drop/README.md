You are given K eggs, and you have access to a building with N floors from 1 to N. 

Each egg is identical in function, and if an egg breaks, you cannot drop it again.

You know that there exists a floor F with 0 <= F <= N such that any egg dropped at a floor higher than F will break, and any egg dropped at or below floor F will not break.

Each move, you may take an egg (if you have an unbroken one) and drop it from any floor X (with 1 <= X <= N). 

Your goal is to know with certainty what the value of F is.

What is the minimum number of moves that you need to know with certainty what F is, regardless of the initial value of F?

**Example 1:**
```
Input: K = 1, N = 2
Output: 2
Explanation: 
Drop the egg from floor 1.  If it breaks, we know with certainty that F = 0.
Otherwise, drop the egg from floor 2.  If it breaks, we know with certainty that F = 1.
If it didn't break, then we know with certainty F = 2.
Hence, we needed 2 moves in the worst case to know what F is with certainty.
```
**Example 2:**
```
Input: K = 2, N = 6
Output: 3
```
**Example 3:**
```
Input: K = 3, N = 14
Output: 4
```

**Note:**

- 1 <= K <= 100
- 1 <= N <= 10000

**Solution:**

```golang
func superEggDrop(K int, N int) int {
  dp := make([][]int, K+1)
  for i := range dp {
    dp[i] = make([]int, N+1)
  }

  var m int
  for dp[K][m] < N {
    m++
    for k := 1; k <= K; k++ {
      dp[k][m] = dp[k][m-1] + dp[k-1][m-1] + 1
    }
  }

  return m
}
```

```golang
func superEggDrop(K, N int) int {
  MIN_INT, MAX_INT := -1 << 31, 1 << 31 - 1
  memo := make([][]int, K + 1)
  for i := range memo {
    memo[i] = make([]int, N + 1)
    for j := range memo[i] {
      memo[i][j] = MIN_INT
    }
  }
  
  var dp func(K, N int) int
  dp = func(K, N int) int {
    if K == 1 {
      return N
    }
    if N == 0 {
      return 0
    }
    if memo[K][N] != MIN_INT {
      return memo[K][N]
    }
    res := MAX_INT
    for i := 1; i <= N; i++ {
      res = min(res, max(dp(K, N - i), dp(K - 1, i - 1)) + 1)
    }

    memo[K][N] = res
    return res
  }
  
  return dp(K, N)
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
