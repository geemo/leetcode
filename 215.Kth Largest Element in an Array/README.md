Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:**

You may assume k is always valid, 1 ≤ k ≤ array's length.

**Solution:**

```golang
func findKthLargest(nums []int, k int) int {
  topK := make([]int, k)
  copy(topK, nums[:k])

  for i := k / 2 - 1; i >= 0; i-- {
    heapify(topK, i)
  }

  for i := k; i < len(nums); i++ {
    if topK[0] < nums[i] {
      topK[0] = nums[i]
      heapify(topK, 0)
    }
  }

  return topK[0]
}

func heapify(arr []int, root int) {
  l, r := root*2+1, root*2+2
  min := root
  size := len(arr)
  if l < size && arr[l] < arr[min] {
    min = l
  }

  if r < size && arr[r] < arr[min] {
    min = r
  }

  if min != root {
    arr[root], arr[min] = arr[min], arr[root]
    heapify(arr, min)
  }
}
```
