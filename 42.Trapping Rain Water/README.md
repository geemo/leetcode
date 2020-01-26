Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![pic](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos for contributing this image!

**Example:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

**Solution:**

```golang
func trap(height []int) int {
	size := len(height)
	if size == 0 {
		return 0
	}

	count := 0
	for i := 1; i < size - 1; i++ {
		lmax, rmax := height[i], height[i]
		for l := 0; l < i; l++ {
			if height[l] > lmax {
				lmax = height[l]
			}
		}

		for r := size - 1; r > i; r-- {
			if height[r] > rmax {
				rmax = height[r]
			}
		}

		count += (min(lmax, rmax) - height[i])
	}

	return count
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

```golang
func trap(heights []int) int {
  var water, lower, level int
  var left, right int = 0, len(heights) - 1
  for left < right {
    if heights[left] < heights[right] {
      lower = heights[left]
      left++
    } else {
      lower = heights[right]
      right--
    }

    if lower > level {
      level = lower
    }
    water += level - lower
  }
  return water
}
```

```golang
func trap(heights []int) int {
  var stack []int
  var water int
  for i := 0; i < len(heights); i++ {
    for len(stack) > 0 && heights[stack[len(stack) - 1]] < heights[i] {
      end := len(stack) - 1
      top := heights[stack[end]]
			stack = stack[:end]
			if len(stack) == 0 {
				break
			}
			peek := stack[len(stack)-1]
			width := i - peek - 1
			water += (min(heights[peek], heights[i]) - top) * width
    }
    stack = append(stack, i)
  }
  return water
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```
