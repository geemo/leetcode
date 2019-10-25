Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Your `KthLargest` class will have a constructor which accepts an integer k and an integer array nums, which contains initial elements from the stream. For each call to the method KthLargest.add, return the element representing the kth largest element in the stream.

**Example:**

```
int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8
```

**Note:**

You may assume that nums' length ≥ k-1 and k ≥ 1.

**Solution:**

```golang
type KthLargest struct {
    data []int
}


func Constructor(k int, nums []int) KthLargest {
  topK := make([]int, 0, k)
  numsLen := len(nums)
  if k <= numsLen {
    topK = append(topK, nums[:k]...)
  } else {
    topK = append(topK, nums...)
  }

  topKLen := len(topK)
  for i := topKLen / 2 - 1; i >= 0; i-- {
    heapify(topK, i, topKLen)
  }

  kl := KthLargest{data: topK}
  for i := k; i < numsLen; i++ {
    kl.Add(nums[i])
  }

  return kl
}


func (this *KthLargest) Add(val int) int {
  dLen, dCap := len(this.data), cap(this.data)
  if dLen < dCap {
    this.data = append(this.data, val)
    for i := (dLen + 1) / 2 - 1; i >= 0; i-- {
      heapify(this.data, i, dLen + 1)
    }
  } else if this.data[0] < val {
    this.data[0] = val
    heapify(this.data, 0, dLen)
  }

  return this.data[0]
}

func heapify(nums []int, root, size int) {
  l, r := root * 2 + 1, root * 2 + 2
  min := root

  if l < size && nums[l] < nums[min] {
    min = l
  }
  if r < size && nums[r] < nums[min] {
    min = r
  }

  if min != root {
    nums[min], nums[root] = nums[root], nums[min]
    heapify(nums, min, size)
  }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * obj := Constructor(k, nums);
 * param_1 := obj.Add(val);
 */
```
