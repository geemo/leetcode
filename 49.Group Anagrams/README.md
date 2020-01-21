Given an array of strings, group anagrams together.

**Example:**
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
**Note:**

- All inputs will be in lowercase.
- The order of your output does not matter.

**Solution:**

```golang
import "sort"
func groupAnagrams(strs []string) [][]string {
  m := make(map[string][]string)
  for _, str := range strs {
    bstr := []uint8(str)
    sort.Slice(bstr, func(i, j int) bool {
      return bstr[i] < bstr[j]
    })
    k := string(bstr)
    m[k] = append(m[k], str)
  }
  ans, i := make([][]string, len(m)), 0
  for k := range m {
    ans[i] = m[k]
    i++
  }
  return ans
}
```
