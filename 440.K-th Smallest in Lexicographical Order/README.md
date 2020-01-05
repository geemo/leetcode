Given integers n and k, find the lexicographically k-th smallest integer in the range from 1 to n.

**Note:** 1 ≤ k ≤ n ≤ 109.

**Example:**
```
Input:
n: 13   k: 2

Output:
10

Explanation:
The lexicographical order is [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], so the second smallest number is 10.
```

**Solution:**

```golang
func findKthNumber(n int, k int) int {
  curr, i := 1, 1
  for i < k {
    cnt := getCount(n, curr, curr + 1)
    if i + cnt > k {
      curr *= 10
      i++
    } else {
      curr++
      i += cnt
    }
  }

  return curr
}

func getCount(n, n1, n2 int) int {
  cnt := 0
  for n1 <= n {
    cnt += min(n2, n + 1) - n1
    n1 *= 10
    n2 *= 10
  }

  return cnt
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}
```
