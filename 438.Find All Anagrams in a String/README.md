Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

**Example 1:**
```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```
**Example 2:**
```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

**Solution:**

```golang
func findAnagrams(s string, p string) []int {
  mp := make(map[uint8]int)
  for i := 0; i < len(p); i++ {
    mp[p[i]]++
  }

  var ans []int
  match, l, ms := 0, 0, make(map[uint8]int)
  for r := 0; r < len(s); r++ {
    ch := s[r]
    if v, ok := mp[ch]; ok {
      ms[ch]++
      if ms[ch] == v {
        match++
      }
    }

    for ; match == len(mp); l++ {
      if (r-l+1) == len(p) {
        ans = append(ans, l)
      }

      ch := s[l]
      if v, ok := mp[ch]; ok {
        ms[ch]--
        if ms[ch] < v {
          match--
        }
      }
    }
  }

  return ans
}
```
