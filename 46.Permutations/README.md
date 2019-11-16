Given a collection of distinct integers, return all possible permutations.

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

**Solution:**

```golang
func permute(nums []int) [][]int {
  numsLen := len(nums)
  var ans [][]int
  isVisited := make(map[int]bool)
  var backtrack func(track []int)
  backtrack = func(track []int) {
    if len(track) == numsLen {
      _track := make([]int, numsLen)
      copy(_track, track)
      ans = append(ans, _track)
    } else {
      for i := 0; i < numsLen; i++ {
        if !isVisited[i] {
          isVisited[i] = true
          track = append(track, nums[i])
          backtrack(track)
          isVisited[i] = false
          track = track[:len(track)-1]
        }
      }
    }
  }
  backtrack([]int{})
  return ans
}
```
