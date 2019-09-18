Given an array of integers `nums`, sort the array in ascending order.

**Example 1:**

```
Input: [5,2,3,1]
Output: [1,2,3,5]
```

**Example 2:**

```
Input: [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

**Note:**

```
1. 1 <= A.length <= 10000
2. -50000 <= A[i] <= 50000
```

**Solution:**

```golang
// heap sort
func heapSort(arr []int) []int {
	size := len(arr)
	if size == 0 {
		return arr
	}

	// build a max heap from the back to the front
	for i := size / 2 - 1; i >= 0; i-- {
		heapify(arr, i, size)
	}

	// swap the top value with the end and reduce the disordered part of the array 
	for i := size - 1; i > 0; i-- {
		arr[0], arr[i] = arr[i], arr[0]
		// refactor heap
		heapify(arr, 0, i)
	}

	return arr
}

func heapify(arr []int, rootIdx, size int) {
	leftIdx, rightIdx := 2*rootIdx+1, 2*rootIdx+2
	maxIdx := rootIdx

	if leftIdx < size && arr[leftIdx] > arr[maxIdx] {
		maxIdx = leftIdx
	}

	if rightIdx < size && arr[rightIdx] > arr[maxIdx] {
		maxIdx = rightIdx
	}

	if maxIdx != rootIdx {
		arr[rootIdx], arr[maxIdx] = arr[maxIdx], arr[rootIdx]
		heapify(arr, maxIdx, size)
	}
}
```
