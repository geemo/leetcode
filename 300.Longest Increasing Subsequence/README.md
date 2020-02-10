Given an unsorted array of integers, find the length of longest increasing subsequence.

**Example:**

```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

**Note:**

- There may be more than one LIS combination, it is only necessary for you to return the length.
- Your algorithm should run in O(n2) complexity.

**Solution:**

```golang
func lengthOfLIS(nums []int) int {
	if len(nums) == 0 {
		return 0
	}

	size := len(nums)
	dp := make([]int, size)
	for i := 0; i < size; i++ {
		dp[i] = 1
	}

	for i := 0; i < size; i++ {
		for j := 0; j < i; j++ {
			if nums[j] < nums[i] {
				dp[i] = max(dp[i], dp[j] + 1)
			}
		}
	}

	lens := 1
	for _, l := range dp {
		lens = max(lens, l)
	}

	return lens
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}
```

binary search

```golang
func lengthOfLIS(nums []int) int {
	var piles int
	top := make([]int, len(nums))
	for _, pocker := range nums {
		left, right := 0, piles
		for left < right {
			mid := left + (right - left) / 2
			if top[mid] >= pocker {
				right = mid
			} else {
				left = mid + 1
			}
		}
		if left == piles {
			piles++
		}
		top[left] = pocker
	}

	return piles
}
```
