Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

**Solution:**

```golang
import "sort"

func merge(intervals [][]int) [][]int {
  n := len(intervals)
  if n == 0 {
    return intervals
  }

  sort.Slice(intervals, func(i, j int) bool {
    return intervals[i][0] < intervals[j][0]
  })

  left := 0
  for right := 1; right < n; right++ {
    a1, a2 := intervals[left][0], intervals[left][1]
    b1, b2 := intervals[right][0], intervals[right][1]

    if b1 > a2 {
      left++
      intervals[left][0] = intervals[right][0]
      intervals[left][1] = intervals[right][1]
    } else {
      intervals[left][0] = min(a1, b1)
      intervals[left][1] = max(a2, b2)
    }
  }

  return intervals[:left+1]
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
