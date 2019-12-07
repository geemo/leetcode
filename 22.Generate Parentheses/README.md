Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

**Solution:**

```golang
func generateParenthesis(n int) []string {
  var ans []string

  var backtrack func(res string, open, close int)
  backtrack = func(res string, open, close int) {
    if open == n && close == n {
      ans = append(ans, res)
      return
    }

    if open < n {
      backtrack(res + "(", open + 1, close)
    }
    if close < open {
      backtrack(res + ")", open, close + 1)
    }
  }
  backtrack("", 0, 0)

  return ans
}
```
