Given a string containing just the characters `'(', ')', '{', '}', '[' and ']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "(]"
Output: false
```

**Example 3:**

```
Input: "()[]{}"
Output: true
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true 
```

**Solution:**

```golang
func isValid(s string) bool {
	if s == "" {
		return true
	}

	str := []byte(s)
	chMap := map[byte]byte{'{': '}', '(': ')', '[': ']'}
	var stack []byte
	for _, ch := range str {
		// push ch onto stack when that is one of the left parentheses
		if _, ok := chMap[ch]; ok {
			stack = append(stack, ch)
			continue
		}
		// pop top of stack if not 
		stackTopIdx := len(stack) - 1
		if stackTopIdx < 0 {
			return false
		}
		stackTop := stack[stackTopIdx]
		if chMap[stackTop] != ch {
			return false
		}
		stack = stack[:stackTopIdx]	
	}
	
	if len(stack) != 0 {
		return false
	}

	return true
}
```
