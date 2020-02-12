Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
Empty cells are indicated by the character '.'.

![img1](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A sudoku puzzle...

![img2](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

...and its solution numbers marked in red.

**Note:**

- The given board contain only digits 1-9 and the character '.'.
- You may assume that the given Sudoku puzzle will have a single unique solution.
- The given board size is always 9x9.

**Solution:**

```golang
func solveSudoku(board [][]byte)  {
  dfs(board)
}

func dfs(board [][]byte) bool {
  for i := 0; i < 9; i++ {
    for j := 0; j < 9; j++ {
      if board[i][j] == '.' {
        for ch := byte('1'); ch <= '9'; ch++ {
          if isValid(board, i, j, ch) {
            board[i][j] = ch
            if dfs(board) {
              return true
            }
            board[i][j] = '.'
          }
        }
        return false
      }
    }
  }
  return true
}

func isValid(board [][]byte, row, col int, ch byte) bool {
  for i := 0; i < 9; i++ {
    if board[row][i] == ch || board[i][col] == ch || board[3*(row/3)+i/3][3*(col/3)+i%3] == ch {
      return false
    }
  }
  return true
}
```
