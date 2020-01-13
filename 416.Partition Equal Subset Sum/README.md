Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

- Each of the array element will not exceed 100.
- The array size will not exceed 200.
 

**Example 1:**
```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**
```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

**Solution:**

```golang
func canPartition(nums []int) bool {
  n := len(nums)
  sum := 0
  for i := 0; i < n; i++ {
    sum += nums[i]
  }

  if sum % 2 == 1 {
    return false
  }
  target := sum / 2
  dp := make([]bool, target + 1)
  dp[0] = true

  for i := 1; i < n; i++ {
    for j := target; j >= nums[i]; j-- {
      dp[j] = dp[j] || dp[j - nums[i]]
    }
  }

  return dp[target]
}
```
