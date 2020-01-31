Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).

**Example 1:**
```
Input: "()())()"
Output: ["()()()", "(())()"]
```
**Example 2:**
```
Input: "(a)())()"
Output: ["(a)()()", "(a())()"]
```
**Example 3:**
```
Input: ")("
Output: [""]
```

**Solutions:**

dfs

```golang
func removeInvalidParentheses(s string) []string {
  var ans []string
  var l, r int
  for i := 0; i < len(s); i++ {
    if s[i] == '(' {
      l++
    } else if s[i] == ')' {
      if l > 0 {
        l--
      } else {
        r++
      }
    }
  }

  dfs(&ans, &s, 0, l, r)
  return ans
}

func dfs(ansp *[]string, sp *string, si, l, r int) {
  if l == 0 && r == 0 {
    if isValid(sp) {
      *ansp = append(*ansp, *sp)
    }
    return
  }

  s := *sp
  for i := si; i < len(s); i++ {
    if i > 0 && s[i] == s[i-1] {
      continue
    }
    if s[i] == '(' && l > 0 {
      str := s[:i] + s[i+1:]
      dfs(ansp, &str, i, l-1, r)
    } else if s[i] == ')' && r > 0 {
      str := s[:i] + s[i+1:]
      dfs(ansp, &str, i, l, r-1)
    }
  }
}

func isValid(sp *string) bool {
  stack, s := 0, *sp
  for i := 0; i < len(s); i++ {
    if s[i] == '(' {
      stack++
    } else if s[i] == ')' {
      stack--
      if stack < 0 {
        return false
      }
    }
  }
  return stack == 0
}
```

bfs

```golang
func removeInvalidParentheses(s string) []string {
  queue := []string{s}
  isVisited := make(map[string]bool)
  isVisited[s] = true
  var ans []string
  var found bool
  for len(queue) != 0 {
    s := queue[0]
    queue = queue[1:]
    if isValid(&s) {
      ans = append(ans, s)
      found = true
    }
    if found {
      continue
    }

    for i := 0; i < len(s); i++ {
      if s[i] != '(' && s[i] != ')' {
        continue
      }
      _s := s[:i] + s[i+1:]
      if !isVisited[_s] {
        isVisited[_s] = true
        queue = append(queue, _s)
      }
    }
  }
  return ans
}

func isValid(sp *string) bool {
  stack, s := 0, *sp
  for i := 0; i < len(s); i++ {
    if s[i] == '(' {
      stack++
    } else if s[i] == ')' {
      stack--
      if stack < 0 {
        return false
      }
    }
  }
  return stack == 0
}
```
