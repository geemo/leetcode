Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
**Note:** You may assume the string contain only lowercase letters.

```golang
func firstUniqChar(s string) int {
  n := len(s)
  var arr [26]int
  for i := 0; i < n; i++ {
    arr[s[i]-'a']++
  }
  for i := 0; i < n; i++ {
    if arr[s[i]-'a'] == 1 {
      return i
    }
  }
  return -1
}
```
