Given a non-empty array of integers, return the k most frequent elements.

**Example 1:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```
**Note:**

- You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
- Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

**Solution:**

```golang
func topKFrequent(nums []int, k int) []int {
  m := make(map[int]int)
  for _, v := range nums {
    m[v]++
  }
  minHeap := CreateMinHeap(k)
  for k, v := range m {
    minHeap.Add(k, v)
  }
  ans := make([]int, k)
  for i := range ans {
    ans[i] = minHeap.PopMin()
  }
  return ans
}

type MinHeap struct {
  data [][2]int
  capacity, count int
}

func CreateMinHeap(capacity int) *MinHeap {
  return &MinHeap{
    data: make([][2]int, capacity),
    capacity: capacity,
    count: 0,
  }
}

func (this *MinHeap) Add(k, v int) {
  if this.count < this.capacity {
    this.data[this.count] = [2]int{k, v}
    this.count++
    this.shiftUp(this.count - 1)
  } else if v > this.Min() {
    this.data[0] = [2]int{k, v}
    this.shiftDown(0)
  }
}

func (this *MinHeap) Min() int {
  if this.count == 0 {
    panic("this heap is empty")
  }
  return this.data[0][1]
}

func (this *MinHeap) PopMin() int {
  if this.count == 0 {
    panic("this heap is empty")
  }
  ret := this.data[0][0]
  this.data[0] = this.data[this.count - 1]
  this.count--
  this.shiftDown(0)

  return ret
}

func (this *MinHeap) shiftUp(i int) {
  for i > 0 && this.data[i][1] < this.data[(i - 1) / 2][1] {
    pi := (i - 1) / 2
    this.data[i], this.data[pi] = this.data[pi], this.data[i]
    i = pi
  }
}

func (this *MinHeap) shiftDown(i int) {
  for i * 2 + 1 < this.count {
    j := i * 2 + 1
    if j + 1 < this.count && this.data[j + 1][1] < this.data[j][1] {
      j++
    }
    if this.data[i][1] <= this.data[j][1] {
      break
    }
    this.data[i], this.data[j] = this.data[j], this.data[i]
    i = j
  }
}
```
