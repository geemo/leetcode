Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Note:**

- You may assume the interval's end point is always bigger than its start point.
- Intervals like [1,2] and [2,3] have borders "touching" but they don't overlap each other.

**Example 1:**

```
Input: [ [1,2], [2,3], [3,4], [1,3] ]

Output: 1

Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.
```

**Example 2:**

```
Input: [ [1,2], [1,2], [1,2] ]

Output: 2

Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.
```

**Example 3:**

```
Input: [ [1,2], [2,3] ]

Output: 0

Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

**Solution:**

```golang
import "sort"

func eraseOverlapIntervals(intervals [][]int) int {
  if len(intervals) == 0 {
    return 0
  }

  sort.Slice(intervals, func(a, b int) bool {
    return intervals[a][1] < intervals[b][1]
  })

  count := 1
  intervalEnd := intervals[0][1]
  for _, interval := range intervals {
    intervalStart := interval[0]
    if intervalStart >= intervalEnd {
      count++
      intervalEnd = interval[1]
    }
  }

  return len(intervals) - count
}
```
