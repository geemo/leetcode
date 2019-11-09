Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

**Example:**

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

**Follow up:**

If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n). 

**Solution:**

```golang
func minSubArrayLen(s int, nums []int) int {
  const MAX_INT = 1<<31 - 1
  minLen := MAX_INT
  l := 0
  sum := 0
  
  for r := 0; r < len(nums); r++ {
    sum += nums[r]
    for sum >= s {
      minLen = min(minLen, r - l + 1)
      sum -= nums[l]
      l++
    }
  }

  if minLen == MAX_INT {
    return 0
  }
  return minLen
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}
```
