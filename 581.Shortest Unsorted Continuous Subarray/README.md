Given an integer array, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the shortest such subarray and output its length.

**Example 1:**
```
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

**Note:**
- Then length of the input array is in range [1, 10,000].
- The input array may contain duplicates, so ascending order here means <=.

**Solution:**

```golang
func findUnsortedSubarray(nums []int) int {
  n := len(nums)
  if n <= 1 {
    return 0
  }
  l, r := 0, n - 1
  for l < r {
    if rangeMin(nums, l, r) >= nums[l] {
      l++
    } else if rangeMax(nums, l, r) <= nums[r] {
      r--
    } else {
      break
    }
  }
  if l == r {
    return 0
  }
  return r - l + 1
}

func rangeMin(nums []int, l, r int) int {
  min := nums[l]
  for i := l + 1; i <= r; i++ {
    if nums[i] < min {
      min = nums[i]
    }
  }
  return min
}

func rangeMax(nums []int, l, r int) int {
  max := nums[r]
  for i := r - 1; i >= l; i-- {
    if nums[i] > max {
      max = nums[i]
    }
  }
  return max
}
```
