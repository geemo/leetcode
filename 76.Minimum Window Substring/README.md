Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note:**

- If there is no such window in S that covers all characters in T, return the empty string "".
- If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

**Solution:**

```golang
func minWindow(s string, t string) string {
    maxInt := 1<<31 - 1
    minLen := maxInt
    l := 0
    start := 0
    tm := make(map[uint8]int)
    for i := 0; i < len(t); i++ {
        tm[t[i]]++
    }

    match := 0
    sm := make(map[uint8]int)
    for r := 0; r < len(s); r++ {
        if v, ok := tm[s[r]]; ok {
            sm[s[r]]++
            if v == sm[s[r]] {
                match++
            }
        }

        for match == len(tm) {
            if minLen > (r - l + 1) {
                minLen = r - l + 1
                start = l
            }

            if tm[s[l]] > 0 {
                sm[s[l]]--
                if sm[s[l]] < tm[s[l]] {
                    match--
                }
            }
            l++
        }
    }

    if minLen == maxInt {
        return ""
    }
    return s[start:start+minLen]
}
```
