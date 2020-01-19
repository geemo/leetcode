Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![pic](https://assets.leetcode.com/uploads/2018/10/12/histogram.png)

Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

![pic2](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.


**Example:**
```
Input: [2,1,5,6,2,3]
Output: 10
```

**Solution:**

```golang
func largestRectangleArea(heights []int) int {
  heights = append([]int{0}, append(heights, 0)...)
  var maxArea int
  var stack []int
  for i := 0; i < len(heights); i++ {
    for len(stack) > 0 && heights[stack[len(stack)-1]] > heights[i] {
      end := len(stack) - 1
      height := heights[stack[end]]
      stack = stack[:end]

      _maxArea := height * (i - stack[len(stack)-1] - 1)
      if _maxArea > maxArea {
        maxArea = _maxArea
      }
    }
    stack = append(stack, i)
  }
  return maxArea
}
```
