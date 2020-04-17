Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters 1 or 0.

**Example 1:**
```
Input: a = "11", b = "1"
Output: "100"
```
**Example 2:**
```
Input: a = "1010", b = "1011"
Output: "10101"
```

**Constraints:**

- Each string consists only of '0' or '1' characters.
- 1 <= a.length, b.length <= 10^4
- Each string is either "0" or doesn't contain any leading zero.

**Solution:**

```golang
import "fmt"

func addBinary(a string, b string) string {
  var ans string
  var carry int
  for i, j := len(a) - 1, len(b) - 1; i >= 0 || j >= 0; {
    sum := carry
    if i >= 0 {
      sum += int(a[i] - '0')
    }
    if j >= 0 {
      sum += int(b[j] - '0')
    }

    carry = sum / 2
    ans = fmt.Sprintf("%d%s", sum % 2, ans)
    i--
    j--
  }
  if carry == 1 {
    ans = "1" + ans
  }
  return ans
}
```
