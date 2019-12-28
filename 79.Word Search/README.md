Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

**Solution:**

```golang
func exist(board [][]byte, word string) bool {
  m := len(board)
  if m == 0 {
    return false
  }
  n := len(board[0])
  if n == 0 {
    return false
  }

  dirs := [4][2]int{[2]int{-1, 0}, [2]int{0, 1}, [2]int{1, 0}, [2]int{0, -1}}
  isVisited := make([][]bool, m)
  for i := range isVisited {
    isVisited[i] = make([]bool, n)
  }
  wordEnd := len(word) - 1
  var backtrack func(i, j, idx int) bool
  backtrack = func(i, j, idx int) bool {
    if idx == wordEnd {
      return board[i][j] == word[idx]
    }

    if board[i][j] == word[idx] {
      isVisited[i][j] = true
      for _, d := range dirs {
        r, c := i + d[0], j + d[1]
        if r < 0 || r >= m || c < 0 || c >= n || isVisited[i][j] {
          continue
        }
        if backtrack(r, c, idx + 1) {
          return true
        }
      }
      isVisited[i][j] = false
    }

    return false
  }

  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      if backtrack(i, j, 0) {
        return true
      }
    }
  }

  return false
}
```
