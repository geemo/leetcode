Given an array A of integers, return true if and only if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i+1 < j with (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])

**Example 1:**
```
Input: A = [0,2,1,-6,6,-7,9,1,2,0,1]
Output: true
Explanation: 0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
```
**Example 2:**
```
Input: A = [0,2,1,-6,6,7,9,-1,2,0,1]
Output: false
```
**Example 3:**
```
Input: A = [3,3,6,5,-2,2,5,1,-9,4]
Output: true
Explanation: 3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
```

**Constraints:**

- 3 <= A.length <= 50000
- -10^4 <= A[i] <= 10^4

**Solution:**

```golang
func canThreePartsEqualSum(A []int) bool {
  n, sum := len(A), 0
  for i := 0; i < n; i++ {
    sum += A[i]
  }

  if sum % 3 != 0 {
    return false
  }
  partSum := sum / 3
  l, r := 0, n - 1
  lsum, rsum := A[l], A[r]
  for l + 1 < r {
    if lsum == partSum && rsum == partSum {
      return true
    }

    if lsum != partSum {
      l++
      lsum += A[l]
    }
    if rsum != partSum {
      r--
      rsum += A[r]
    }
  }

  return false
}
```
