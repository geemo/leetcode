Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.

 

**Example:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```

**Note:**

- The number of tasks is in the range [1, 10000].
- The integer n is in the range [0, 100].

**Solution:**

```golang
import "sort"

func leastInterval(tasks []byte, n int) int {
  tlen := len(tasks)
  if tlen <= 1 || n == 0 {
    return tlen
  }
  letCnt := make([]int, 26)
  for i := 0; i < tlen; i++ {
    letCnt[tasks[i] - 'A']++
  }
  sort.Ints(letCnt)

  maxCnt := letCnt[25]
  ans := (maxCnt - 1) * (n + 1) + 1
  for i := 24; i >= 0 && letCnt[i] == maxCnt; i-- {
    ans++
  }

  if tlen > ans {
    return tlen
  }
  return ans
}
```
