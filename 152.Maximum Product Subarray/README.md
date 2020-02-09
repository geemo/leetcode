Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example 1:**
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
**Example 2:**
```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

**Solution:**

```golang
func maxProduct(nums []int) int {
  n := len(nums)
  if n == 1 {
    return nums[0]
  }

  dp := make([][2]int, n)
  if nums[0] < 0 {
    dp[0][1] = nums[0]
  } else {
    dp[0][0] = nums[0]
  }

  var ans int = dp[0][0]
  for i := 1; i < n; i++ {
    if nums[i] > 0 {
      dp[i][0] = max(dp[i-1][0] * nums[i], nums[i])
      dp[i][1] = min(dp[i-1][1] * nums[i], nums[i])
    } else {
      dp[i][1] = min(dp[i-1][0] * nums[i], nums[i])
      dp[i][0] = max(dp[i-1][1] * nums[i], nums[i])
    }
    ans = max(ans, dp[i][0])
  }

  return ans
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}
```
