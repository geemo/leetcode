Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.


**Example:**
```
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

**Note:**

- All inputs are consist of lowercase letters a-z.
- The values of words are distinct.

**Solution:**

```golang
func findWords(board [][]byte, words []string) []string {
  m, n := len(board), len(board[0])
  trie := CreateTrie()
  for _, word := range words {
    trie.Add([]byte(word))
  }

  var ans []string
  isVisited := make([][]bool, m)
  for i := range isVisited {
    isVisited[i] = make([]bool, n)
  }
  dirs := [4][2]int{[2]int{-1, 0}, [2]int{1, 0}, [2]int{0, -1}, [2]int{0, 1}}
  var backtrack func(i, j int, w []byte, root *TrieNode)
  backtrack = func(i, j int, w []byte, root *TrieNode) {
    char := board[i][j]
    node, ok := root.Next[char]
    if !ok {
      return
    }

    isVisited[i][j] = true
    w = append(w, char)
    defer func(r, c int) {
      isVisited[r][c] = false
      w = w[:len(w)-1]
    } (i, j)
    if node.IsWord && !node.IsVisited {
      node.IsVisited = true
      ans = append(ans, string(w))
    }

    for _, d := range dirs {
      r, c := i + d[0], j + d[1]
      if r < 0 || r >= m || c < 0 || c >= n || isVisited[r][c] {
        continue
      }
      backtrack(r, c, w, node)
    }
  }

  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      backtrack(i, j, []byte{}, trie.root)
    }
  }

  return ans
}

type Trie struct {
  root *TrieNode
}

type TrieNode struct {
  IsWord bool
  IsVisited bool
  Next map[byte]*TrieNode
}

func CreateTrie() *Trie {
  return &Trie{
    root: &TrieNode{
      Next: make(map[byte]*TrieNode),
    },
  }
}

func (this *Trie) Add(word []byte) {
  cur := this.root
  for _, c := range word {
    if _, ok := cur.Next[c]; !ok {
      cur.Next[c] = &TrieNode{Next: make(map[byte]*TrieNode)}
    }
    cur = cur.Next[c]
  }
  cur.IsWord = true
}
```
