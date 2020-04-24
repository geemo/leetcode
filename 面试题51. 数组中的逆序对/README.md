在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**
```
输入: [7,5,6,4]
输出: 5
```

**Solution:**

```golang
func reversePairs(nums []int) int {
  n := len(nums)
  if n <= 1 {
    return 0
  }

  _nums, tmp := make([]int, n), make([]int, n)
  copy(_nums, nums)

  return _reversePairs(_nums, tmp, 0, n - 1)
}

func _reversePairs(nums, tmp []int, left, right int) int {
  if left >= right {
    return 0
  }
  
  mid := left + (right - left) / 2
  leftPairs := _reversePairs(nums, tmp, left, mid)
  rightPairs := _reversePairs(nums, tmp, mid + 1, right)
  if nums[mid] <= nums[mid + 1] {
    return leftPairs + rightPairs
  }
  
  crossPairs := mergeAndCount(nums, tmp, left, mid, right)
  
  return leftPairs + rightPairs + crossPairs
}

func mergeAndCount(nums, tmp []int, left, mid, right int) int {
  for i := left; i <= right; i++ {
    tmp[i] = nums[i]
  }
  
  i, j, count := left, mid + 1, 0
  for k := left; k <= right; k++ {
    if i == mid + 1 {
      nums[k] = tmp[j]
      j++
    } else if j == right + 1 {
      nums[k] = tmp[i]
      i++
    } else if tmp[i] <= tmp[j] {
      nums[k] = tmp[i]
      i++
    } else {
      nums[k] = tmp[j]
      j++
      count += (mid - i + 1)
    }
  }
  
  return count
}
```
