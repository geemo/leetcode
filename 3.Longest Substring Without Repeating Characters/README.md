Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Solution:**

```golang
func lengthOfLongestSubstring(s string) int {
  if s == "" {
    return 0
  }

  bStr := []byte(s)
  hash := [256]int{}
  l, count := 0, 0
  for r := 0; r < len(bStr); r++ {
    hash[bStr[r]]++
    for hash[bStr[r]] != 1 {
      hash[bStr[l]]--
      l++
    }
    count = max(count, l-r+1)
  }

  return count
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
