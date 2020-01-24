Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example:**
```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
**Note:**

You can assume that you can always reach the last index.

**Solution:**

```golang
func jump(nums []int) int {
  n, end, maxPos, step := len(nums), 0, 0, 0
  for i := 0; i < n - 1; i++ {
    maxPos = max(maxPos, nums[i] + i)
    if i == end {
      end = maxPos
      step++
    }
  }
  return step
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
