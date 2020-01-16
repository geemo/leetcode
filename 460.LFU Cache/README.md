Design and implement a data structure for Least Frequently Used (LFU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie (i.e., two or more keys that have the same frequency), the least recently used key would be evicted.

Note that the number of times an item is used is the number of calls to the get and put functions for that item since it was inserted. This number is set to zero when the item is removed.

 

**Follow up:**
Could you do both operations in O(1) time complexity?

 

**Example:**
```
LFUCache cache = new LFUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.get(3);       // returns 3.
cache.put(4, 4);    // evicts key 1.
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

**Solution:**

```golang
type LFUCache struct {
  cache map[int]*Node
  freq map[int]*DoubleList
  ncap, size, minFreq int
}

func Constructor(capacity int) LFUCache {
  return LFUCache {
    cache: make(map[int]*Node),
    freq: make(map[int]*DoubleList),
    ncap: capacity,
  }
}

func (this *LFUCache) Get(key int) int {
  if node, ok := this.cache[key]; ok {
    this.IncFreq(node)
    return node.val
  }
  return -1
}

func (this *LFUCache) Put(key, value int) {
  if this.ncap == 0 {
    return
  }
  if node, ok := this.cache[key]; ok {
    node.val = value
    this.IncFreq(node)
  } else {
    if this.size >= this.ncap {
      node := this.freq[this.minFreq].RemoveLast()
      delete(this.cache, node.key)
      this.size--
    }
    x := &Node{key: key, val: value, freq: 1}
    this.cache[key] = x
    if this.freq[1] == nil {
      this.freq[1] = CreateDL()
    }
    this.freq[1].AddFirst(x)
    this.minFreq = 1
    this.size++
  }
}

func (this *LFUCache) IncFreq(node *Node) {
  _freq := node.freq
  this.freq[_freq].Remove(node)
  if this.minFreq == _freq && this.freq[_freq].IsEmpty() {
    this.minFreq++
    delete(this.freq, _freq)
  }

  node.freq++
  if this.freq[node.freq] == nil {
    this.freq[node.freq] = CreateDL()
  }
  this.freq[node.freq].AddFirst(node)
}

type DoubleList struct {
  head, tail *Node
}

type Node struct {
  prev, next *Node
  key, val, freq int
}

func CreateDL() *DoubleList {
  head, tail := &Node{}, &Node{}
  head.next, tail.prev = tail, head
  return &DoubleList {
    head: head,
    tail: tail,
  }
}

func (this *DoubleList) AddFirst(node *Node) {
  node.next = this.head.next
  node.prev = this.head

  this.head.next.prev = node
  this.head.next = node
}

func (this *DoubleList) Remove(node *Node) {
  node.prev.next = node.next
  node.next.prev = node.prev

  node.next = nil
  node.prev = nil
}

func (this *DoubleList) RemoveLast() *Node {
  if this.IsEmpty() {
    return nil
  }

  last := this.tail.prev
  this.Remove(last)

  return last
}

func (this *DoubleList) IsEmpty() bool {
  return this.head.next == this.tail
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```
