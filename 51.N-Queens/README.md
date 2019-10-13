The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

![nqueue](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

**Solution:**

```golang
func solveQueens(n int) [][]string {
	board := make([][]byte, n)
	for i := range board {
		board[i] = make([]byte, n)
		for j := range board[i] {
			board[i][j] = '.'
		}
	}

	var ans [][]string
	var backtrace func(int)
	backtrace = func(row int) {
		if row == n {
			ans = append(ans, boardStr(board))
		} else {
			for col := 0; col < n; col++ {
				if !isValid(board, row, col) {
					continue
				}
				board[row][col] = 'Q'
				backtrace(row + 1)
				board[row][col] = '.'
			}
		}
	}
	backtrace(0)

	return ans
}

func boardStr(board [][]byte) []string {
	var ret []string
	for _, line := range board {
		ret = append(ret, string(line))
	}
	return ret
}

func isValid(board [][]byte, row, col int) bool {
	for i := row - 1; i >= 0; i-- {
		if board[i][col] == 'Q' {
			return false
		}
	}

	for r, c := row - 1, col - 1; r >= 0 && c >= 0; {
		if board[r][c] == 'Q' {
			return false
		}
		r--
		c--
	}

	for r, c := row - 1, col + 1; r >= 0  && c < len(board); {
		if board[r][c] == 'Q' {
			return false
		}
		r--
		c++
	}

	return true
}
```
