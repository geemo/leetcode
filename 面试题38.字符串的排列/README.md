输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

**示例:**
```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

**限制：**

- 1 <= s 的长度 <= 8

**Solution:**

```golang
func permutation(s string) []string {
  n := len(s)
  if n == 0 {
    return nil
  }

  var ans []string
  var dfs func(si int, bs []byte)
  dfs = func(si int, bs []byte) {
    if si == n {
      ans = append(ans, string(bs))
      return
    }

    m := make(map[byte]bool)
    for i := si; i < n; i++ {
      if m[bs[i]] {
        continue
      }
      m[bs[i]] = true
      bs[i], bs[si] = bs[si], bs[i]
      dfs(si + 1, bs)
      bs[i], bs[si] = bs[si], bs[i]
    }
  }
  dfs(0, []byte(s))

  return ans
}
```
