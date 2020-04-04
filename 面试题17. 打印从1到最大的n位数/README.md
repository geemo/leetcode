输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

**示例 1:**
```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

**说明：**

- 用返回一个整数列表来代替打印
- n 为正整数

**Solution:**

```golang
func printNumbers(n int) []int {
  p := pow(n)
  ans := make([]int, p - 1)
  for i := 0; i < p - 1; i++ {
    ans[i] = i + 1
  }
  return ans
}

func pow(n int) int {
  ans := 1
  for ; n > 0; n-- {
    ans = ans * 10
  }
  return ans
}
```
