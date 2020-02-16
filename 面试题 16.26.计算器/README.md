给定一个包含正整数、加(+)、减(-)、乘(*)、除(/)的算数表达式(括号除外)，计算其结果。

表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

**示例 1:**
```
输入: "3+2*2"
输出: 7
```
**示例 2:**
```
输入: " 3/2 "
输出: 1
```
**示例 3:**
```
输入: " 3+5 / 2 "
输出: 5
```
**说明：**

- 你可以假设所给定的表达式都是有效的。
- 请不要使用内置的库函数 eval。

**Solution:**

```golang
func calculate(s string) int {
  s += "+"
  var sign uint8 = '+'
  var num int
  var stack []int
  for i := 0; i < len(s); i++ {
    c := s[i]
    if c == ' ' {
      continue
    } else if c >= '0' && c <= '9' {
      num = num * 10 + int(c - '0')
    } else {
      if sign == '+' {
        stack = append(stack, num)
      } else if sign == '-' {
        stack = append(stack, -num)
      } else {
        end := len(stack) - 1
        preNum := stack[end]
        stack = stack[:end]
        if sign == '*' {
          stack = append(stack, preNum * num)
        } else {
          stack = append(stack, preNum / num)
        }
      }
      sign = c
      num = 0
    }
  }

  var ans int
  for _, v := range stack {
    ans += v
  }
  return ans
}
```
