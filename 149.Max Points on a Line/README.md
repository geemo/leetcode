Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Example 1:**
```
Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
```
**Example 2:**
```
Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```
**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

**Solution:**

```golang
import "fmt"

func maxPoints(points [][]int) int {
  n := len(points)
  if n <= 1 {
    return n
  }

  var ans int
  for i := 0; i < n - 1; i++ {
    repeat, tmpMax := 1, 0
    slope := make(map[string]int)
    for j := i + 1; j < n; j++ {
      dy := points[i][1] - points[j][1]
      dx := points[i][0] - points[j][0]
      if dy == 0 && dx == 0 {
        repeat++
        continue
      }

      g := gcd(dy, dx)
      dy /= g
      dx /= g
      k := fmt.Sprintf("%d/%d", dy, dx)
      slope[k]++
      tmpMax = max(tmpMax, slope[k])
    }
    ans = max(ans, repeat + tmpMax)
  }

  return ans
}

func gcd(dy, dx int) int {
  for dx != 0 {
    dx, dy = dy % dx, dx
  }
  return dy
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
