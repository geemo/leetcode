Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.

**Example 1:**
```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```
**Example 2:**
```
Input: [[7,10],[2,4]]
Output: 1
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

**Solution:**

```golang
import "sort"

func minMeetingRooms(intervals [][]int) int {
  n := len(intervals)
  if n == 0 {
    return 0
  }

  sort.Slice(intervals, func(i, j int) bool {
    return intervals[i][0] < intervals[j][0]
  })

  minHeap := CreateMinHeap(n)
  for i := 0; i < n; i++ {
    minHeap.Add(intervals[i][1])
    if intervals[i][0] < minHeap.Min() {
      continue
    }
    minHeap.Pop()
  }

  return minHeap.Size()
}

type MinHeap struct {
  data []int
  count int
}

func CreateMinHeap(capacity int) *MinHeap {
  return &MinHeap {
    data: make([]int, capacity),
    count: 0,
  }
}

func (this *MinHeap) Min() int {
  if this.count == 0 {
    panic("this heap is empty")
  }
  return this.data[0]
}

func (this *MinHeap) Add(num int) {
  this.data[this.count] = num
  this.count++
  this.shiftUp(this.count - 1)
}

func (this *MinHeap) Pop() int {
  if this.count == 0 {
    panic("this heap is empty")
  }

  ret := this.data[0]
  this.data[0] = this.data[this.count - 1]
  this.count--
  this.shiftDown(0)

  return ret
}

func (this *MinHeap) Size() int {
  return this.count
}

func (this *MinHeap) shiftUp(i int) {
  for i > 0 && this.data[i] < this.data[(i - 1) / 2] {
    pi := (i - 1) / 2
    this.data[i], this.data[pi] = this.data[pi], this.data[i]
    i = pi
  }
}

func (this *MinHeap) shiftDown(i int) {
  for i * 2 + 1 < this.count {
    j := i * 2 + 1
    if j + 1 < this.count && this.data[j + 1] < this.data[j] {
      j++
    }

    if this.data[i] <= this.data[j] {
      break
    }
    this.data[i], this.data[j] = this.data[j], this.data[i]
    i = j
  }
}
```
