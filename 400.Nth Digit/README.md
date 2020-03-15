Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

**Note:**

n is positive and will fit within the range of a 32-bit signed integer (n < 231).

**Example 1:**
```
Input:
3

Output:
3
```
**Example 2:**
```
Input:
11

Output:
0

Explanation:
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
```

**Solution:**

```golang
import "strconv"

func findNthDigit(n int) int {
  num, digit := 9, 1
  for n - num * digit > 0 {
    n -= num * digit
    num *= 10
    digit++
  }

  div, mod := divmod(n - 1, digit)
  ans := strconv.Itoa(tenPow(digit - 1) + div)[mod]
  return int(ans - '0')
}

func divmod(a, b int) (int, int) {
  return a / b, a % b
}

func tenPow(b int) int {
  a := 1
  if b == 0 {
    return a
  }

  for ; b > 0; b-- {
    a *= 10
  }

  return a
}
```
