Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

**Example:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Note:** 
You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

**Follow up:**
Could you solve it in linear time?

**Solution:**

```golang
type MonotoneQueue struct {
    data []int
}

func (this *MonotoneQueue) Push(n int) {
    for len(this.data) != 0 && this.data[len(this.data)-1] < n {
        this.data = this.data[:len(this.data)-1]
    }
    this.data = append(this.data, n)
}

func (this *MonotoneQueue) Pop(n int) {
    if len(this.data) != 0 && this.data[0] == n {
        this.data = this.data[1:]
    }
}

func (this *MonotoneQueue) Max() int {
    return this.data[0]
}

func maxSlidingWindow(nums []int, k int) []int {
    queue := &MonotoneQueue{}
    var res []int
    
    for i := 0; i < len(nums); i++ {
        if i < k-1 {
            queue.Push(nums[i])
        } else {
            queue.Push(nums[i])
            res = append(res, queue.Max())
            queue.Pop(nums[i-k+1])
        }
    }

    return res
}
```
