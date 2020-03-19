Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

**Note:**
Assume the length of given string will not exceed 1,010.

**Example:**
```
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

**Solution:**

```golang
func longestPalindrome(s string) int {
  n := len(s)
  var count [128]int
  for i := 0; i < n; i++ {
    count[s[i]]++
  }

  var ans int
  for i := 65; i < 128; i++ {
    ans += count[i] / 2 * 2
    if ans % 2 == 0 && count[i] % 2 == 1 {
      ans++
    }
  }

  return ans
}
```
