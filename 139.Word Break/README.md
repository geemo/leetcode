Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.
**Example 1:**
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```
**Example 2:**
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```
**Example 3:**
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

**Solution:**

```golang
func wordBreak(s string, wordDict []string) bool {
  return dfs(CreateTrie(wordDict).Root, make(map[int]bool), &s, 0)
}

func dfs(root *TrieNode, visited map[int]bool, sp *string, idx int) bool {
  s := *sp
  slen := len(s)
  if idx == slen {
    return true
  }

  visited[idx] = true
  node := root
  for i := idx; i < slen; i++ {
    node = node.Next[s[i]]
    if node == nil {
      return false
    }

    if node.IsWord && !visited[i + 1] && dfs(root, visited, sp, i + 1) {
      return true
    }
  }

  return false
}

type Trie struct {
  Root *TrieNode
}

type TrieNode struct {
  Next map[uint8]*TrieNode
  IsWord bool
}

func CreateTrie(wordDict []string) *Trie {
  trie := &Trie{
    Root: &TrieNode{
      Next: make(map[uint8]*TrieNode),
    },
  }

  for i := 0; i < len(wordDict); i++ {
    trie.Add(&wordDict[i])
  }

  return trie
}

func (this *Trie) Add(sp *string) {
  s := *sp
  slen := len(s)

  node := this.Root
  for i := 0; i < slen; i++ {
    c := s[i]
    if node.Next[c] == nil {
      node.Next[c] = &TrieNode{
        Next: make(map[uint8]*TrieNode),
      }
    }
    node = node.Next[c]
  }

  node.IsWord = true
}
```
