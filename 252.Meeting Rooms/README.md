Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), determine if a person could attend all meetings.

**Example 1:**
```
Input: [[0,30],[5,10],[15,20]]
Output: false
```
**Example 2:**
```
Input: [[7,10],[2,4]]
Output: true
```
**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

**Solution:**

```golang
import "sort"

func canAttendMeetings(intervals [][]int) bool {
  n := len(intervals)
  if n == 0 {
    return true
  }
  sort.Slice(intervals, func(i, j int) bool {
    return intervals[i][0] < intervals[j][0]
  })

  for i := 0; i < n - 1; i++ {
    if intervals[i][1] > intervals[i+1][0] {
      return false
    }
  }
  return true
}
```
