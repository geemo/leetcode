You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

**Example 1:**

```
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Note:**

You may assume that you have an infinite number of each kind of coin.

**Solution:**

```golang
func coinChange(coins []int, amount int) int {
  if len(coins) == 0 {
    return -1
  }

  if amount == 0 {
    return 0
  }

  dp := make([]int, amount + 1)
  for i := 1; i < amount + 1; i++ {
    if contains(coins, i) {
      dp[i] = 1
      continue
    }
    dp[i] = amount + 1
  }

  for i := 1; i <= amount; i++ {
    for _, c := range coins {
      if i >= c {
        dp[i] = min(dp[i], dp[i-c] + 1)
      }
    }
  }

  if dp[amount] == amount + 1 {
    return -1
  }

  return dp[amount]
}

func min(a, b int) int {
  if a < b {
    return a
  }

  return b
}

func contains(arr []int, target int) bool {
  for _, val := range arr {
    if val == target {
      return true
    }
  }

  return false
}
```
