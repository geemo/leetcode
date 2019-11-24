Given two strings `s1` and `s2`, write a function to return true if `s2` contains the permutation of `s1`. In other words, one of the first string's permutations is the substring of the second string.

 

**Example 1:**

```
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

**Note:**

- The input strings only contain lower case letters.
- The length of both given strings is in range [1, 10,000].

**Solution:**

```golang
func checkInclusion(s1 string, s2 string) bool {
  len1, len2 := len(s1), len(s2)
  m1, m2 := make(map[uint8]int), make(map[uint8]int)
  for i := 0; i < len1; i++ {
    m1[s1[i]]++
  }

  left, count := 0, 0
  for right := 0; right < len2; right++ {
    c1 := s2[right]
    if v, ok := m2[c1]; ok {
      m2[c1]++
      if m1[c1] == v {
        count++
      }
    }

    for count == len(m1) {
      if (right - left + 1) == len1 {
        return true
      }

      c2 := s2[left]
      if v, ok := m1[c2]; ok {
        m2[c2]--
        if m2[c2] < v {
          count--
        }
      }
      left++
    }
  }

  return false
}
```
