Implement a MapSum class with insert, and sum methods.

For the method insert, you'll be given a pair of (string, integer). The string represents the key and the integer represents the value. If the key already existed, then the original key-value pair will be overridden to the new one.

For the method sum, you'll be given a string representing the prefix, and you need to return the sum of all the pairs' value whose key starts with the prefix.

**Example 1:**
```
Input: insert("apple", 3), Output: Null
Input: sum("ap"), Output: 3
Input: insert("app", 2), Output: Null
Input: sum("ap"), Output: 5
```

**Solution:**

```golang
type MapSum struct {
  root *TrieNode
}

type TrieNode struct {
  Val int
  Next map[uint8]*TrieNode
}


/** Initialize your data structure here. */
func Constructor() MapSum {
  return MapSum{
    root: &TrieNode{Next: make(map[uint8]*TrieNode)},
  }
}


func (this *MapSum) Insert(key string, val int)  {
  n := len(key)
  cur := this.root
  for i := 0; i < n; i++ {
    c := key[i]
    if _, ok := cur.Next[c]; !ok {
      cur.Next[c] = &TrieNode{Next: make(map[uint8]*TrieNode)}
    }
    cur = cur.Next[c]
  }
  cur.Val = val
}


func (this *MapSum) Sum(prefix string) int {
  n := len(prefix)
  cur := this.root
  for i := 0; i < n; i++ {
    c := prefix[i]
    if _, ok := cur.Next[c]; !ok {
      return 0
    }
    cur = cur.Next[c]
  }

  return sum(cur)
}

func sum(node *TrieNode) int {
  if node == nil {
    return 0
  }
  ans := node.Val
  for _, n := range node.Next {
    ans += sum(n)
  }
  return ans
}


/**
 * Your MapSum object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(key,val);
 * param_2 := obj.Sum(prefix);
 */
```
