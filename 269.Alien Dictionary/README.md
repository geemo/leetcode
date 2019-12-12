There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of non-empty words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

**Example 1:**
```
Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: "wertf"
```
**Example 2:**
```
Input:
[
  "z",
  "x"
]

Output: "zx"
```
**Example 3:**
```
Input:
[
  "z",
  "x",
  "z"
] 

Output: "" 

Explanation: The order is invalid, so return "".
```
**Note:**

- You may assume all letters are in lowercase.
- You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
- If the order is invalid, return an empty string.
- There may be multiple valid order of letters, return any one of them is fine.

**Solution:**

```golang
func alienOrder(words []string) string {
  n := len(words)
  if n == 0 {
    return ""
  }

  m := make(map[uint8]map[uint8]bool)
  for i := 0; i < n - 1; i++ {
    for j := 0; j < len(words[i]) && j < len(words[i + 1]); j++ {
      c, c1 := words[i][j], words[i + 1][j]
      if c == c1 {
        continue
      }
      set := m[c]
      if set == nil {
        set = make(map[uint8]bool)
      }
      set[c1] = true
      m[c] = set
      break
    }
  }

  var inDegree [26]int
  for i := range inDegree {
    inDegree[i] = -1
  }
  for i := 0; i < n; i++ {
    for j := 0; j < len(words[i]); j++ {
      inDegree[words[i][j] - 'a'] = 0
    }
  }
  for _, s := range m {
    for c := range s {
      inDegree[c - 'a']++
    }
  }

  var queue []uint8
  var count int
  for k, v := range inDegree {
    if v != -1 {
      count++
    }
    if v == 0 {
      queue = append(queue, uint8(k + 'a'))
    }
  }
  var ans []uint8
  for len(queue) != 0 {
    c := queue[0]
    queue = queue[1:]
    ans = append(ans, c)
    if v, ok := m[c]; ok {
      for c1 := range v {
        inDegree[c1 - 'a']--
        if inDegree[c1 - 'a'] == 0 {
          queue = append(queue, c1)
        }
      }
    }
  }

  if len(ans) != count {
    return ""
  }
  return string(ans)
}
```
