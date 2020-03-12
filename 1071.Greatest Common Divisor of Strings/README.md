For strings S and T, we say "T divides S" if and only if S = T + ... + T  (T concatenated with itself 1 or more times)

Return the largest string X such that X divides str1 and X divides str2.

**Example 1:**
```
Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"
```
**Example 2:**
```
Input: str1 = "ABABAB", str2 = "ABAB"
Output: "AB"
```
**Example 3:**
```
Input: str1 = "LEET", str2 = "CODE"
Output: ""
```

**Note:**

- 1 <= str1.length <= 1000
- 1 <= str2.length <= 1000
- str1[i] and str2[i] are English uppercase letters.

**Solution:**

```golang
func gcdOfStrings(str1 string, str2 string) string {
  if str1 + str2 != str2 + str1 {
    return ""
  }

  return str1[:gcd(len(str1), len(str2))]
}

func gcd(a, b int) int {
  if b == 0 {
    return a
  }
  return gcd(b, a % b)
}
```
