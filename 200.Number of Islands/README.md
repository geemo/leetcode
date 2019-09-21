Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```
Input:
11000
11000
00100
00011

Output: 3
```

**Soulation:**

```golang
// dfs
func numIslands(grid [][]byte) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	row, col := len(grid), len(grid[0])
	count := 0
	var dfs func(int, int)
	dfs = func(r, c int) {
		if r < 0 || r >= row || c < 0 || c >= col || grid[r][c] != '1' {
			return
		}	

		grid[r][c] = '2'

		dfs(r-1, c)
		dfs(r+1, c)
		dfs(r, c-1)
		dfs(r, c+1)
	}

	for i := 0; i < row; i++ {
		for j := 0; j < col; j++ {
			if grid[i][j] == '1' {
				dfs(i, j)
				count++
			}
		}
	}

	return count
}
```
