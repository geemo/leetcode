A robot on an infinite grid starts at point (0, 0) and faces north.  The robot can receive one of three possible types of commands:

- -2: turn left 90 degrees
- -1: turn right 90 degrees
- 1 <= x <= 9: move forward x units
Some of the grid squares are obstacles. 

The i-th obstacle is at grid point `(obstacles[i][0], obstacles[i][1])`

If the robot would try to move onto them, the robot stays on the previous grid square instead (but still continues following the rest of the route.)

Return the square of the maximum Euclidean distance that the robot will be from the origin.

**Example 1:**
```
Input: commands = [4,-1,3], obstacles = []
Output: 25
Explanation: robot will go to (3, 4)
```
**Example 2:**
```
Input: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
Output: 65
Explanation: robot will be stuck at (1, 4) before turning left and going to (1, 8)
``` 
**Note:**

- 0 <= commands.length <= 10000
- 0 <= obstacles.length <= 10000
- -30000 <= obstacle[i][0] <= 30000
- -30000 <= obstacle[i][1] <= 30000
- The answer is guaranteed to be less than 2 ^ 31.

**Solution:**

```golang
import "fmt"

func robotSim(commands []int, obstacles [][]int) int {
  dir := [4][2]int{[2]int{0,1}, [2]int{1,0}, [2]int{0,-1}, [2]int{-1,0}}
  m := make(map[string]bool)
  for _, obs := range obstacles {
    m[fmt.Sprintf("%d,%d", obs[0], obs[1])] = true
  }

  var ans, didx, x, y int
  for _, cmd := range commands {
    if cmd == -2 {
      didx = (didx + 3) % 4
    } else if cmd == -1 {
      didx = (didx + 1) % 4
    } else {
      dx, dy := dir[didx][0], dir[didx][1]
      for i := 1; i <= cmd; i++ {
        if !m[fmt.Sprintf("%d,%d", x + dx, y + dy)] {
          x += dx
          y += dy
          ans = max(ans, x * x + y * y)
        }
      }
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
```
