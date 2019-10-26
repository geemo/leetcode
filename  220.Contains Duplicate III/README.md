Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between `nums[i]` and `nums[j]` is at most `t` and the absolute difference between `i` and `j` is at most `k`.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

**Solution:**

```golang
// Sliding window
func containsNearbyAlmostDuplicate(nums []int, k int, t int) bool {
  for i := 1; i < len(nums); i++ {
    for j := max(i - k, 0); j < i; j++ {
      if abs(nums[i] - nums[j]) <= t {
        return true
      }
    }
  }

  return false
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}

func abs(n int) int {
  if n < 0 {
    return -n
  }
  return n
}
```
