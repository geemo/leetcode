Given a positive integer n, find the least number of perfect square numbers (for example, `1, 4, 9, 16, ...`) which sum to n.

**Example 1:**

```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

**Solutions:**

```golang
// bfs
func numSquares(n int) int {
  /* init the queue to: 
   * [][2]int{
   *   [2]int{val, step}, 
   *   ...
   * }
   */
	queue := [][2]int{
		[2]int{n, 0},
	}
  // length n+1 in order to make the index map the value n
	visisted := make([]bool, n+1)

	for len(queue) != 0 {
		val, step := queue[0][0], queue[0][1]
		queue = queue[1:]

		for i := 1; ;i++ {
      // adjacent node
			remainder := val - i*i

			if remainder < 0 {
				break
			}

			if remainder == 0 {
				return step + 1
			}

			if !visisted[remainder] {
        // save adjacent node
        queue = append(queue, [2]int{remainder, step + 1})
				visisted[remainder] = true
			}

		}
	}

	return -1
}

```
