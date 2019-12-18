Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

**Example:**
```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
**Note:**

- The array is only modifiable by the update function.
- You may assume the number of calls to update and sumRange function is distributed evenly.

**Solution:**

```golang
type NumArray struct {
  root *SegTreeNode
  nums []int
}

func Constructor(nums []int) NumArray {
  return NumArray{
    root: buildTree(nums, 0, len(nums) - 1),
    nums: nums,
  }
}

func (this *NumArray) Update(i, val int) {
  if i < 0 || i >= len(this.nums) {
    return
  }
  this.root.Update(i, val)
}

func (this *NumArray) SumRange(i, j int) int {
  if i > j {
    return 0
  }
  if i < 0 {
    i = 0
  }
  if j >= len(this.nums) {
    j = len(this.nums)
  }
  return this.root.Query(i, j)
}

type SegTreeNode struct {
  start, end, sum int
  left, right *SegTreeNode
}

func buildTree(nums []int, start, end int) *SegTreeNode {
  if start > end {
    return nil
  }
  root := &SegTreeNode{start: start, end: end, sum: nums[start]}
  if start == end {
    return root
  }

  mid := start + (end - start) / 2
  root.left = buildTree(nums, start, mid)
  root.right = buildTree(nums, mid + 1, end)
  root.sum = root.left.sum + root.right.sum

  return root
}

func (this *SegTreeNode) Update(pos, val int) {
  if this.start == this.end {
    this.sum = val
    return
  }

  mid := this.start + (this.end - this.start) / 2
  if pos <= mid {
    this.left.Update(pos, val)
  } else {
    this.right.Update(pos, val)
  }
  this.sum = this.left.sum + this.right.sum
}

func (this *SegTreeNode) Query(start, end int) int {
  if this.start == start && this.end == end {
    return this.sum
  }

  mid := this.start + (this.end - this.start) / 2
  if end <= mid {
    return this.left.Query(start, end)
  } else if start >= mid + 1 {
    return this.right.Query(start, end)
  }
  return this.left.Query(start, mid) + this.right.Query(mid + 1, end)
}
```
