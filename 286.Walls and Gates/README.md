You are given a m x n 2D grid initialized with these three possible values.

1. -1 - A wall or an obstacle.
2. 0 - A gate.
3. INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

**Example:**

Given the 2D grid:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

After running your function, the 2D grid should be:

```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

**Solutions:**

```golang
// bfs 
func wallsAndGates(rooms [][]int) {
	if len(rooms) == 0 || len(rooms[0]) == 0 {
		return
	}

	DIRECTIONS := [4][2]int{
		[2]int{-1, 0},
		[2]int{1, 0},
		[2]int{0, -1},
		[2]int{0, 1},
	}

	rLen, cLen := len(rooms), len(rooms[0])
	var gates [][2]int
	for i := 0; i < rLen; i++ {
		for j := 0; j < cLen; j++ {
			if rooms[i][j] == 0 {
				gates = append(gates, [2]int{i, j})
			}
		}
	}

	for _, gate := range gates {
		queue := [][2]int{gate}
		for len(queue) != 0 {
			row, col := queue[0][0], queue[0][1]
			queue = queue[1:]

			for _, direct := range DIRECTIONS {
				r, c := row+direct[0], col+direct[1]
				if r < 0 || r >= rLen || c < 0 || c >= cLen ||  rooms[r][c] <= rooms[row][col]+1 {
					continue
				}

				rooms[r][c] = rooms[row][col] + 1
				queue = append(queue, [2]int{r, c})
			}
		}
	}
}

```

```golang
// dfs
func wallsAndGates(rooms [][]int) {
  fmt.Println(rooms)
	if len(rooms) == 0 || len(rooms[0]) == 0 {
		return
	}

	m, n := len(rooms), len(rooms[0])
	var dfs func(int, int, int)
	dfs = func(i, j, step int) {
		if i < 0 || i >= m || j < 0 || j >= n || rooms[i][j] < step {
			return
		}

		rooms[i][j] = step
		dfs(i-1, j, step+1)
		dfs(i+1, j, step+1)
		dfs(i, j-1, step+1)
		dfs(i, j+1, step+1)
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if rooms[i][j] == 0 {
				dfs(i, j, 0)
			}
		}
	}
}
```
