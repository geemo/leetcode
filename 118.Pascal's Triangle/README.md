Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.


In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

**Solution:**

```golang
func generate(numRows int) [][]int {
  var ans [][]int
  for i := 1; i <= numRows; i++ {
    arr, preRow := make([]int, i), i - 2
    arr[0], arr[i - 1] = 1, 1
    for j := 1; j < i - 1; j++ {
      arr[j] = ans[preRow][j-1] + ans[preRow][j]
    }
    ans = append(ans, arr)
  }
  return ans
}
```
