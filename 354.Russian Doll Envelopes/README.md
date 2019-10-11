You have a number of envelopes with widths and heights given as a pair of integers `(w, h)`. One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

**Note:**
Rotation is not allowed.

**Example:**

```
Input: [[5,4],[6,4],[6,7],[2,3]]
Output: 3 
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

**Solution:**

```golang
import "sort"

func maxEnvelopes(envelopes [][]int) int {
	size := len(envelopes)
	if size == 0 {
		return 0
	}

	sort.Slice(envelopes, func(i, j int) bool {
		if envelopes[i][0] == envelopes[j][0] {
			return envelopes[i][1] > envelopes[j][1]
		}
		return envelopes[i][0] < envelopes[j][0]
	})

	heights := make([]int, size)
	for i, envelop := range envelopes {
		heights[i] = envelop[1]
	}

	return lengthOfLIS(heights)
}

func lengthOfLIS(heights []int) int {
	heapCount := 0
	size := len(heights)
	top := make([]int, size)
	
	for i := 0; i < size; i++ {
		left, right := 0, heapCount
		for left < right {
			mid := (left + right) / 2
			if top[mid] > heights[i] {
				right = mid
			} else if top[mid] < heights[i] {
				left = mid + 1
			} else {
				right = mid
			}
		}

		if left == heapCount {
			heapCount++
		}
		top[left] = heights[i]
	}

	return heapCount
}
```
