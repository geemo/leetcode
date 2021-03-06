Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**
```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```
**Example 2:**
```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```
**Example 3:**
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

**Solution:**

```golang
// 回溯递归
func maxProfit(prices []int) int {
  var ans int
  n := len(prices)
  // status: true 持有，false 不持有
  var dfs func(i int, status bool, profit int)
  dfs = func(i int, status bool, profit int) {
    if i == n {
      if profit > ans {
        ans = profit
      }
      return
    }

    dfs(i + 1, status, profit)
    if status {
      dfs(i + 1, false, profit + prices[i])
    } else {
      dfs(i + 1, true, profit - prices[i])
    }
  }
  dfs(0, false, 0)
  return ans
}
```

二维dp
```golang
func maxProfit(prices []int) int {
  n := len(prices)
  if n <= 1 {
    return 0
  }
  dp := make([][2]int, n)
  dp[0][0] = 0
  dp[0][1] = -prices[0]

  for i := 1; i < n; i++ {
    dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
    dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
  }
  return dp[n-1][0]
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```

一维dp
```golang
func maxProfit(prices []int) int {
  n := len(prices)
  if n <= 1 {
    return 0
  }

  dp_i_0, dp_i_1 := 0, -prices[0]
  for i := 1; i < n; i++ {
    dp_i_0 = max(dp_i_0, dp_i_1 + prices[i])
    dp_i_1 = max(dp_i_1, dp_i_0 - prices[i])
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

贪心
```golang
func maxProfit(prices []int) int {
  var ans int
  for i := 1; i < len(prices); i++ {
    if prices[i-1] < prices[i] {
      ans += prices[i] - prices[i-1]
    }
  }
  return ans
}
```
