输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

**示例 1：**
```
输入：target = 9
输出：[[2,3,4],[4,5]]
```
**示例 2：**
```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

**限制：**

- 1 <= target <= 10^5

**Solution:**

```golang
func findContinuousSequence(target int) [][]int {
  l, sum := 1, 0
  var ans [][]int
  for r := 1; r < target; r++ {
    sum += r
    for sum > target {
      sum -= l
      l++
    }
    if sum == target {
      ans = append(ans, getRange(l, r))
    }
  }
  return ans
}

func getRange(l, r int) []int {
  ans := make([]int, r - l + 1)
  for i := l; i <= r; i++ {
    ans[i - l] = i
  }
  return ans
}
```
