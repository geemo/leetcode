Say you have an array for which the i-th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.

**Note:**
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**Example 1:**

```
Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

**Example 2:**

```
Input: [3,2,6,5,0,3], k = 2
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.
             Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

**Solution:**

```golang
func maxProfit(k int, prices []int) int {
  size := len(prices)
  if k > size / 2 {
    return maxProfitInf(prices)
  }

  dp := make([][][2]int, size)
  for i := range dp {
    dp[i] = make([][2]int, k + 1)
  }

  for i := 0; i < size; i++ {
    for j := 1; j <= k; j++ {
      if i - 1 < 0 {
        dp[i][j][0] = 0
        dp[i][j][1] = -prices[i]
        continue
      }

      dp[i][j][0] = max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i])
      dp[i][j][1] = max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i])
    }
  }

  return dp[size - 1][k][0]
}

func maxProfitInf(prices []int) int {
  size := len(prices)
  if size <= 1 {
    return 0
  }

  dp_i_0, dp_i_1 := 0, -prices[0]
  for i := 0; i < size; i++ {
    tmp := dp_i_0
    dp_i_0 = max(dp_i_0, dp_i_1 + prices[i])
    dp_i_1 = max(dp_i_1, tmp - prices[i])
  }

  return dp_i_0
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
