Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*'.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).
```
**Note:**

- s could be empty and contains only lowercase letters a-z.
- p could be empty and contains only lowercase letters a-z, and characters like ? or *.

**Example 1:**
```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```
**Example 2:**
```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```
**Example 3:**
```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```
**Example 4:**
```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```
**Example 5:**
```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

**Solution:**

```golang
func isMatch(s string, p string) bool {
  slen, plen := len(s), len(p)
  dp := make([][]bool, slen + 1)
  for i := range dp {
    dp[i] = make([]bool, plen + 1)
  }
  dp[0][0] = true
  for i := 1; i <= plen; i++ {
    if p[i - 1] == '*' {
      dp[0][i] = dp[0][i - 1]
    }
  }

  for i := 1; i <= slen; i++ {
    for j := 1; j <= plen; j++ {
      if s[i - 1] == p[j - 1] || p[j - 1] == '?' {
        dp[i][j] = dp[i - 1][j - 1]
      } else if p[j - 1] == '*' {
        dp[i][j] = dp[i - 1][j] || dp[i][j - 1]
      }
    }
  }

  return dp[slen][plen]
}
```
