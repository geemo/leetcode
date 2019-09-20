Evaluate the value of an arithmetic expression in ![Reverse Polish Notation](en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

**Note:**

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

**Solution:**

```golang
import "strconv"

func evalRPN(tokens []string) int {
	if len(tokens) == 0 {
		return 0
	}

	var stack []int
	for _, token := range tokens {
		switch token {
		case "+", "-", "*", "/":
			idx := len(stack) - 2
			// pop up the last two number
			a, b := stack[idx], stack[idx+1]
			stack = stack[:idx]
			res := eval(a, b, token)
			// push result into the stack
			stack = append(stack, res)
			continue
		default:
			i, _ := strconv.Atoi(token)
			stack = append(stack, i) 
		}
	}

	return stack[0]
}

func eval(a, b int, operand string) int {
	var res int
	switch operand {
	case "+":
		res = a + b
	case "-":
		res = a - b
	case "*":
		res = a * b
	case "/":
		res = a / b
	}

	return res
}
```
