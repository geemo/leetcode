Given an array of integers nums sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Solution:**

```golang
func searchRange(nums []int, target int) []int {
	left := leftBound(nums, target)
	right := rightBound(nums, target)

	return []int{left, right}
}

func leftBound(nums []int, target int) int {
	size := len(nums)
	if size == 0 {
		return -1
	}

	left, right := 0, size
	for left < right {
		mid := (left + right) / 2
		if nums[mid] == target {
			right = mid
		} else if nums[mid] < target {
			left = mid + 1
		} else if nums[mid] > target {
			right = mid
		}
	}

	if left == size {
		return -1
	}

	if nums[left] == target {
		return left
	}

	return -1
}

func rightBound(nums []int, target int) int {
	size := len(nums)
	if size == 0 {
		return -1
	}

	left, right := 0, size
	for left < right {
		mid := (left + right) / 2
		if nums[mid] == target {
			left = mid + 1
		} else if nums[mid] < target {
			left = mid + 1
		} else if nums[mid] > target {
			right = mid
		}
	}

	if left == 0 {
		return -1
	}

	if nums[left - 1] == target {
		return left - 1
	}

	return -1
}
```
