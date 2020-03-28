Given a list of words, we may encode it by writing a reference string S and a list of indexes A.

For example, if the list of words is ["time", "me", "bell"], we can write it as S = "time#bell#" and indexes = [0, 2, 5].

Then for each index, we will recover the word by reading from the reference string from that index until we reach a "#" character.

What is the length of the shortest reference string S possible that encodes the given words?

**Example:**
```
Input: words = ["time", "me", "bell"]
Output: 10
Explanation: S = "time#bell#" and indexes = [0, 2, 5].
```

**Note:**

- 1 <= words.length <= 2000.
- 1 <= words[i].length <= 7.
- Each word has only lowercase letters.

**Solution:**

```golang
import "sort"
func minimumLengthEncoding(words []string) int {
  n := len(words)
  if n == 0 {
    return 0
  }
  sort.Slice(words, func(i, j int) bool {
    return len(words[i]) > len(words[j])
  })
  trie := CreateTrie()
  var lens int
  for _, word := range words {
    lens += trie.Add([]byte(word))
  }

  return lens
}

type Trie struct {
  root *TrieNode
}

type TrieNode struct {
  Next map[byte]*TrieNode
}

func CreateTrie() *Trie {
  return &Trie{
    root: &TrieNode{
      Next: make(map[byte]*TrieNode),
    },
  }
}

func (this *Trie) Add(word []byte) int {
  n := len(word)
  cur := this.root
  var isNew bool
  for i := n - 1; i >= 0; i-- {
    ch := word[i]
    if _, ok := cur.Next[ch]; !ok {
      isNew = true
      cur.Next[ch] = &TrieNode{Next: make(map[byte]*TrieNode)}
    }
    cur = cur.Next[ch]
  }

  if isNew {
    return len(word) + 1
  }
  return 0
}
```
