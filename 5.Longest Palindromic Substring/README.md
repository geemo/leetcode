Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

**Solution:**

dp

```golang
func longestPalindrome(s string) string {
  sLen := len(s)
  if sLen <= 1 {
    return s
  }

  longestPalind := 1
  longestPalindStr := s[0:1]
  dp := make([][]bool, sLen)
  for i := range dp {
    dp[i] = make([]bool, sLen)
  }

  for r := 1; r < sLen; r++ {
    for l := 0; l < r; l++ {
      if s[l] == s[r] && (r - l <= 2 || dp[l + 1][r - 1]) {
        dp[l][r] = true
        if r - l + 1 > longestPalind {
          longestPalind = r - l + 1
          longestPalindStr = s[l:r+1]
        }
      }
    }
  }

  return longestPalindStr
}
```

dp optimize

```golang
func longestPalindrome(s string) string {
  n := len(s)
  if n == 0 {
    return ""
  }

  dp := make([][]bool, n)
  for i := range dp {
    dp[i] = make([]bool, n)
    dp[i][i] = true
  }

  var l, r int
  for k := 1; k < n; k++ {
    i, j := 0, k
    for i < n && j < n {
      if s[i] == s[j] && (j-i <= 2 || dp[i+1][j-1]) {
        dp[i][j] = true
        if j-i > r-l {
          l, r = i, j
        }
      }
      i++
      j++
    }
  }

  return s[l:r+1]
}
```

two pointers

```golang
func longestPalindrome(s string) string {
  n := len(s)
  if n == 0 {
    return ""
  }

  var lps func(l, r int) string
  lps = func(l, r int) string {
    for l >= 0 && r < n && s[l] == s[r] {
      l--
      r++
    }
    return s[l+1:r]
  }

  var ans string
  for i := 0; i < n; i++ {
    s1, s2 := lps(i, i), lps(i, i+1)
    if len(s1) > len(ans) {
      ans = s1
    }
    if len(s2) > len(ans) {
      ans = s2
    }
  }

  return ans
}
```

```golang
func longestPalindrome(s string) string {
  n := len(s)
  if n == 0 {
    return ""
  }

  var lps func(l, r int) int
  lps = func(l, r int) int {
    for l >= 0 && r < n && s[l] == s[r] {
      l--
      r++
    }
    return r - l - 1
  }

  si, ei := 0, 0
  for i := 0; i < n; i++ {
    l1, l2 := lps(i, i), lps(i, i + 1)
    l := max(l1, l2)
    if l > (ei - si + 1) {
      si = i - (l - 1) / 2
      ei = i + l / 2
    }
  }

  return s[si:ei+1]
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
