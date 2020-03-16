Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

**Example 1:**
```
Input: "aabcccccaaa"
Output: "a2b1c5a3"
```
**Example 2:**
```
Input: "abbccd"
Output: "abbccd"
Explanation: 
The compressed string is "a1b2c2d1", which is longer than the original string.
```

**Note:**

- 0 <= S.length <= 50000

**Solution:**

```golang
import "fmt"

func compressString(s string) string {
  n := len(s)
  if n <= 2 {
    return s
  }

  var ans string
  l, r, cnt := 0, 0, 0
  for ; r < n; r++ {
    if s[l] == s[r] {
      cnt++
    } else {
      ans += fmt.Sprintf("%c%d", s[l], cnt)
      l = r
      cnt = 1
    }
  }
  ans += fmt.Sprintf("%c%d", s[l], cnt)

  if len(ans) > n {
    return s
  }
  return ans
}
```
