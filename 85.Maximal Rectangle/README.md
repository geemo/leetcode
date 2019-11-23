Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

**Example:**

```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

**Solution:**

```golang
func maximalRectangle(matrix [][]byte) int {
  row := len(matrix)
  if row == 0 {
    return 0
  }
  col := len(matrix[0])
  var maxArea int
  heights := make([]int, col)
  for i := 0; i < row; i++ {
    for j := 0; j < col; j++ {
      if matrix[i][j] == byte('1') {
        heights[j] += 1
      } else {
        heights[j] = 0
      }
    }
    maxArea = max(maxArea, largestRectangleArea(heights))
  }

  return maxArea
}

func largestRectangleArea(heights []int) int {
  newHeights := make([]int, len(heights) + 2)
  for i := 1; i < len(newHeights) - 1; i++ {
    newHeights[i] = heights[i - 1]
  }

  var maxArea int
  var stack []int
  for i := 0; i < len(newHeights); i++ {
    for len(stack) != 0 && newHeights[stack[len(stack) - 1]] > newHeights[i] {
      end := len(stack) - 1
      cur := stack[end]
      stack = stack[:end]

      maxArea = max(maxArea, (i - stack[len(stack) - 1] - 1) * newHeights[cur])
    }
    stack = append(stack, i)
  }

  return maxArea
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
