Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:**

```
Input: 19
Output: true
Explanation: 
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
12 + 02 + 02 = 1
```

**Solution:**

```golang
import "strconv"

func isHappy(n int) bool {
  isVisited := make(map[int]bool)
  isVisited[1] = true

  for !isVisited[n] {
    isVisited[n] = true
    str := strconv.Itoa(n)
    sum := 0
    for i := 0; i < len(str); i++ {
      num := int(str[i] - '0')
      sum += num * num
    }
    n = sum
  }

  return n == 1
}
```
