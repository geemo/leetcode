Given two lists of `closed` intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

(Formally, a closed interval `[a, b]` (with `a <= b`) denotes the set of real numbers x with a <= x <= b.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of [1, 3] and [2, 4] is [2, 3].)


**Example 1:**

![intervals](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```
Input: A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
Reminder: The inputs and the desired output are lists of Interval objects, and not arrays or lists.
```

**Note:**

- 0 <= A.length < 1000
- 0 <= B.length < 1000
- 0 <= A[i].start, A[i].end, B[i].start, B[i].end < 10^9

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.


**Solution:**

```golang
func intervalIntersection(A [][]int, B [][]int) [][]int {
  i, j := 0, 0
  lenA, lenB := len(A), len(B)
  var ans [][]int

  for i < lenA && j < lenB {
    a1, a2 := A[i][0], A[i][1]
    b1, b2 := B[j][0], B[j][1]

    if a2 >= b1 && b2 >= a1 {
      ans = append(ans, []int{max(a1, b1), min(a2, b2)})
    }

    if a2 < b2 {
      i++
    } else {
      j++
    }
  }

  return ans
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}
```
