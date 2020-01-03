Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**
```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```
**Example 2:**
```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

**Solution:**

```golang
func longestValidParentheses(s string) int {
  if s == "" {
    return 0
  }

  ans, n := 0, len(s)
  dp := make([]int, n)
  for i := 1; i < n; i++ {
    if s[i] == ')' {
      if s[i - 1] == '(' {
        if i >= 2 {
          dp[i] = dp[i - 2] + 2
        } else {
          dp[i] = 2
        }
      } else if s[i - 1] == ')' && i - dp[i - 1] - 1 >= 0 && s[i - dp[i - 1] - 1] == '(' {
        if i - dp[i - 1] - 2 >= 0 {
          dp[i] = dp[i - 1] + 2 + dp[i - dp[i - 1] - 2]
        } else {
          dp[i] = dp[i - 1] + 2
        }
      }

      if dp[i] > ans {
        ans = dp[i]
      }
    }
  }

  return ans
}
```
