Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

**Follow up:**

Could you do both operations in O(1) time complexity?

**Example:**

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

**Solution:**

```golang
type LRUCache struct {
    Map map[int]*Node
    Cache *DoubleList
    Cap int
}


func Constructor(capacity int) LRUCache {
  return LRUCache{
    Map: make(map[int]*Node),
    Cache: DoubleListConstructor(),
    Cap: capacity,
  }
}


func (this *LRUCache) Get(key int) int {
  node, ok := this.Map[key]
  if !ok {
    return -1
  }
  this.Cache.Remove(node)
  this.Cache.AddFirst(node)
  return node.Val
}


func (this *LRUCache) Put(key int, value int)  {
  x := &Node{Key: key, Val: value}
  if node, ok := this.Map[key]; ok {
    this.Cache.Remove(node)
    this.Cache.AddFirst(x)
    this.Map[key] = x
  } else {
    if this.Cache.GetSize() >= this.Cap {
      last := this.Cache.RemoveLast()
      delete(this.Map, last.Key)
    }
    this.Cache.AddFirst(x)
    this.Map[key] = x
  }
}

type DoubleList struct {
  Head, Tail *Node
  Size int
}

type Node struct {
  Prev, Next *Node
  Key, Val int
}

func DoubleListConstructor() *DoubleList {
  head, tail := &Node{}, &Node{}
  head.Next = tail
  tail.Prev = head

  return &DoubleList{Head: head, Tail: tail}
}

func (this *DoubleList) AddFirst(x *Node) {
  x.Next = this.Head.Next
  x.Prev = this.Head

  x.Next.Prev = x
  this.Head.Next = x
  this.Size++
}

func (this *DoubleList) Remove(x *Node) {
  if this.Head.Next == this.Tail  {
    return
  }
  x.Prev.Next = x.Next
  x.Next.Prev = x.Prev

  x.Prev = nil
  x.Next = nil

  this.Size--
}

func (this *DoubleList) RemoveLast() *Node {
  if this.Head.Next == this.Tail {
    return nil
  }
  last := this.Tail.Prev
  this.Remove(last)

  return last
}

func (this *DoubleList) GetSize() int {
  return this.Size
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```
