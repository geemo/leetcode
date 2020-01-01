Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

- You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
- 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

**Example:**
```
Input: [3,1,5,8]
Output: 167 
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

**Solution:**

```golang
func maxCoins(nums []int) int {
  newNums := append([]int{1}, append(nums, 1)...)
  n := len(newNums)
  dp := make([][]int, n)
  for i := range dp {
    dp[i] = make([]int, n)
  }
  dp[0][0], dp[n - 1][n - 1] = 1, 1

  return maxCoin(newNums, 0, n - 1, dp)
}

func maxCoin(newNums []int, start, end int, dp [][]int) int {
  if end - start <= 1 {
    return 0
  }
  if dp[start][end] != 0 {
    return dp[start][end]
  }
  mc := 0
  for i := start + 1; i < end; i++ {
    mc = max(mc, maxCoin(newNums, start, i, dp) + newNums[start]*newNums[i]*newNums[end] + maxCoin(newNums, i, end, dp))
  }
  dp[start][end] = mc
  return mc
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
