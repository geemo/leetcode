给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

**示例:**
```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**提示：**

- 你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

**Solution:**

```golang
func maxSlidingWindow(nums []int, k int) []int {
  var ans []int
  mq := &MQ{}
  for i, num := range nums {
    mq.Push(num)
    if i >= k - 1 {
      ans = append(ans, mq.Max())
      mq.Pop(nums[i - k + 1])
    }
  }

  return ans
}

type MQ struct {
  data []int
}

func (this *MQ) Push(num int) {
  for len(this.data) > 0 && this.data[len(this.data) - 1] < num {
    this.data = this.data[:len(this.data) - 1]
  }
  this.data = append(this.data, num)
}

func (this *MQ) Pop(num int) {
  if len(this.data) > 0 && this.data[0] == num {
    this.data = this.data[1:]
  }
}

func (this *MQ) Max() int {
  if len(this.data) == 0 {
    panic("mq is empty")
  }
  return this.data[0]
}
```
