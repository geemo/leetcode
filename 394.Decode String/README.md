Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like `3a` or `2[4]`.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

**Solution:**

```golang
func decodeString(s string) string {
	bStr := []byte(s)

	var stack []byte
	for _, ch := range bStr {
		if ch == ']' {
			var encBytes []byte
			var kBytes []byte
			// parse encBytes
			for len(stack) > 0 {
				endIdx := len(stack) - 1
				endCh := stack[endIdx]
				stack = stack[:endIdx]
				if endCh == '[' {
					break
				}
				encBytes = append([]byte{endCh}, encBytes...)
			}

			// parse kBytes
			for len(stack) > 0 {
				endIdx := len(stack) - 1
				endCh := stack[endIdx]
				if endCh < '0' || endCh > '9' {
					break
				}
				kBytes = append([]byte{endCh}, kBytes...)
				stack = stack[:endIdx]
			}
			// append encBytes to stack
			k, _ := strconv.Atoi(string(kBytes))
			for i := 0; i < k; i++ {
				stack = append(stack, encBytes...)
			}
			continue
		}

		stack = append(stack, ch)
	}

	return string(stack)
}

```
