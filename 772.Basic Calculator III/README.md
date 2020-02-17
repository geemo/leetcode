Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

The expression string contains only non-negative integers, +, -, *, / operators , open ( and closing parentheses ) and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of [-2147483648, 2147483647].

**Some examples:**
```
"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
```

**Note:** Do not use the eval built-in library function.

**Solution:**
```golang
func calculate(s string) int {
  slen := len(s)
  var calc func(i int) (int, int)
  calc = func(i int) (int, int) {
    var sign uint8 = '+'
    var stack []int
    var num int
    for ; i < slen; i++ {
      c := s[i]
      isDigit := c >= '0' && c <= '9'
      if isDigit {
        num = num * 10 + int(c - '0')
      }

      if c == '(' {
        num, i = calc(i + 1)
      }

      if !isDigit && c != ' ' || i == slen - 1 {
        switch sign {
          case '+': stack = append(stack, num)
          case '-': stack = append(stack, -num)
          case '*': stack[len(stack)-1] *= num
          case '/': stack[len(stack)-1] /= num
        }
        sign = c
        num = 0
      }

      if c == ')' {
        break
      }
    }

    var ans int
    for _, v := range stack {
      ans += v
    }
    return ans, i
  }

  ans, _ := calc(0)
  return ans
}
```
