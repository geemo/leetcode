Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

**Example 1:**
```
Input: "3+2*2"
Output: 7
```
**Example 2:**
```
Input: " 3/2 "
Output: 1
```
**Example 3:**
```
Input: " 3+5 / 2 "
Output: 5
```
**Note:**

- You may assume that the given expression is always valid.
- Do not use the eval built-in library function.

**Solution:**

```golang
func calculate(s string) int {
  s += "+"
  var stack []int
  var tmp int
  var sign uint8 = '+'

  for i := 0; i < len(s); i++ {
    c := s[i]
    if c == ' ' {
      continue
    } else if c >= '0' && c <= '9' {
      tmp = tmp * 10 + int(c - '0')
    } else {
      if sign == '+' {
        stack = append(stack, tmp)
      } else if sign == '-' {
        stack = append(stack, -tmp)
      } else {
        end := len(stack) - 1
        preNum := stack[end]
        stack = stack[:end]
        if sign == '*' {
          stack = append(stack, preNum * tmp)
        } else {
          stack = append(stack, preNum / tmp)
        }
      }
      sign = c
      tmp = 0
    }
  }

  var ans int
  for _, v := range stack {
    ans += v
  }

  return ans
}
```
