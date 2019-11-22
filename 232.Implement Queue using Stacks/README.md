Implement the following operations of a queue using stacks.

- push(x) -- Push element x to the back of queue.
- pop() -- Removes the element from in front of queue.
- peek() -- Get the front element.
- empty() -- Return whether the queue is empty.

**Example:**

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

**Notes:**

- You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
- You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

**Solution:**

```golang
type MyQueue struct {
  stackPush []int
  stackPop  []int
}


/** Initialize your data structure here. */
func Constructor() MyQueue {
  return MyQueue{}
}

func (this *MyQueue) PushToPop() {
  if len(this.stackPop) == 0 {
    for len(this.stackPush) != 0 {
      end := len(this.stackPush) - 1
      this.stackPop = append(this.stackPop, this.stackPush[end])
      this.stackPush = this.stackPush[:end]
    }
  }
}

/** Push element x to the back of queue. */
func (this *MyQueue) Push(x int)  {
  this.stackPush = append(this.stackPush, x)
  this.PushToPop()
}


/** Removes the element from in front of queue and returns that element. */
func (this *MyQueue) Pop() int {
  if this.Empty() {
    panic("queue is empty")
  }
  this.PushToPop()
  end := len(this.stackPop) - 1
  ans := this.stackPop[end]
  this.stackPop = this.stackPop[:end]
  return ans
}


/** Get the front element. */
func (this *MyQueue) Peek() int {
  if this.Empty() {
    panic("queue is empty")
  }
  this.PushToPop()
  return this.stackPop[len(this.stackPop) - 1]
}


/** Returns whether the queue is empty. */
func (this *MyQueue) Empty() bool {
  return len(this.stackPush) == 0 && len(this.stackPop) == 0
}


/**
 * Your MyQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 * param_3 := obj.Peek();
 * param_4 := obj.Empty();
 */
```
