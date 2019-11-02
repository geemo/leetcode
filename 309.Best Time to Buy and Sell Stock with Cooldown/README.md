Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

**Solution:**

```golang
func maxProfit(prices []int) int {
  size := len(prices)
  if size <= 1 {
    return 0
  }

  dp_i_0, dp_i_1, dp_pre_0 := 0, -1 << 31, 0
  for i := 0; i < size; i++ {
    tmp := dp_i_0
    dp_i_0 = max(dp_i_0, dp_i_1 + prices[i])
    dp_i_1 = max(dp_i_1, dp_pre_0 - prices[i])
    dp_pre_0 = tmp
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
