Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```

search(word) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

**Example:**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**Note:**

You may assume that all words are consist of lowercase letters `a-z`.

**Solution:**

```golang
type WordDictionary struct {
  Val uint8
  Children map[uint8]*WordDictionary
  IsEnd bool
}

func Constructor() WordDictionary {
  return WordDictionary{IsEnd: true, Children: make(map[uint8]*WordDictionary)}
}

func (this *WordDictionary) AddWord(word string) {
  root := this
  for i := 0; i < len(word); i++ {
    ch := word[i]
    if root.Children[ch] == nil {
      root.Children[ch] = &WordDictionary{Val: ch, Children: make(map[uint8]*WordDictionary)}
    }
    root = root.Children[ch]
  }
  root.IsEnd = true
}

func (this *WordDictionary) Search(word string) bool {
  if word == "" || this.Children == nil {
    return this.IsEnd
  }

  ch := word[0]
  if this.Children[ch] != nil {
    return this.Children[ch].Search(word[1:])
  }

  if ch == '.' {
    for k := range this.Children[ch] {
      if this.Children[k].Search(word[1:]) {
        return true
      }
    }
  }

  return false
}
```
