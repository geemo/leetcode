Implement `strStr()`.

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's `strstr()` and Java's `indexOf()`.

**Solution:**

```golang
// kmp
func strStr(haystack string, needle string) int {
	if needle == "" {
		return 0
	}

	if haystack == "" {
		return -1
	}

	m := len(needle)
	dp := make([][256]int, m)
	dp[0][needle[0]] = 1
	x := 0
	for i := 1; i < m; i++ {
		for j := 0; j < 256; j++ {
			dp[i][j] = dp[x][j]	
		}
		dp[i][needle[i]] = i + 1
		x = dp[x][needle[i]]
	}

	s := 0
	for i, c := range haystack {
		s = dp[s][c]
		if s == m {
			return i - m + 1
		}
	}

	return -1
}
```
