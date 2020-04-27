Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

**Example 1:**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Solution:**

```golang
func search(nums []int, target int) int {
  n := len(nums)
  l, r := 0, n - 1
  for l < r {
    m := l + (r - l) / 2
    if nums[m] >= nums[0] && (target > nums[m] || target < nums[0]) {
      l = m + 1
    } else if target > nums[m] && target < nums[0] {
      l = m + 1
    } else {
      r = m
    }
  }
  if l == r && nums[l] == target {
    return l
  }
  return -1
}
```
