Design your implementation of the circular double-ended queue (deque).

Your implementation should support following operations:

- MyCircularDeque(k): Constructor, set the size of the deque to be k.
- insertFront(): Adds an item at the front of Deque. Return true if the operation is successful.
- insertLast(): Adds an item at the rear of Deque. Return true if the operation is successful.
- deleteFront(): Deletes an item from the front of Deque. Return true if the operation is successful.
- deleteLast(): Deletes an item from the rear of Deque. Return true if the operation is successful.
- getFront(): Gets the front item from the Deque. If the deque is empty, return -1.
- getRear(): Gets the last item from Deque. If the deque is empty, return -1.
- isEmpty(): Checks whether Deque is empty or not. 
- isFull(): Checks whether Deque is full or not.
 

**Example:**
```
MyCircularDeque circularDeque = new MycircularDeque(3); // set the size to be 3
circularDeque.insertLast(1);			// return true
circularDeque.insertLast(2);			// return true
circularDeque.insertFront(3);			// return true
circularDeque.insertFront(4);			// return false, the queue is full
circularDeque.getRear();  			// return 2
circularDeque.isFull();				// return true
circularDeque.deleteLast();			// return true
circularDeque.insertFront(4);			// return true
circularDeque.getFront();			// return 4
``` 

**Note:**

- All values will be in the range of [0, 1000].
- The number of operations will be in the range of [1, 1000].
- Please do not use the built-in Deque library.

**Solution:**

```golang
type MyCircularDeque struct {
  data []int
  capacity int
  front, last int
}


/** Initialize your data structure here. Set the size of the deque to be k. */
func Constructor(k int) MyCircularDeque {
  return MyCircularDeque{
    data: make([]int, k + 1),
    capacity: k + 1,
  }    
}


/** Adds an item at the front of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) InsertFront(value int) bool {
  if this.IsFull() {
    return false
  }
  this.front = (this.front - 1 + this.capacity) % this.capacity
  this.data[this.front] = value
  return true
}


/** Adds an item at the rear of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) InsertLast(value int) bool {
  if this.IsFull() {
    return false
  }
  this.data[this.last] = value
  this.last = (this.last + 1) % this.capacity
  return true
}


/** Deletes an item from the front of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) DeleteFront() bool {
  if this.IsEmpty() {
    return false
  }
  this.front = (this.front + 1) % this.capacity
  return true
}


/** Deletes an item from the rear of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) DeleteLast() bool {
  if this.IsEmpty() {
    return false
  }
  this.last = (this.last - 1 + this.capacity) % this.capacity
  return true
}


/** Get the front item from the deque. */
func (this *MyCircularDeque) GetFront() int {
  if this.IsEmpty() {
    return -1
  }
  return this.data[this.front]
}


/** Get the last item from the deque. */
func (this *MyCircularDeque) GetRear() int {
  if this.IsEmpty() {
    return -1
  }
  return this.data[(this.last - 1 + this.capacity) % this.capacity]
}


/** Checks whether the circular deque is empty or not. */
func (this *MyCircularDeque) IsEmpty() bool {
  return this.front == this.last
}


/** Checks whether the circular deque is full or not. */
func (this *MyCircularDeque) IsFull() bool {
  return (this.last + 1) % this.capacity == this.front
}


/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * obj := Constructor(k);
 * param_1 := obj.InsertFront(value);
 * param_2 := obj.InsertLast(value);
 * param_3 := obj.DeleteFront();
 * param_4 := obj.DeleteLast();
 * param_5 := obj.GetFront();
 * param_6 := obj.GetRear();
 * param_7 := obj.IsEmpty();
 * param_8 := obj.IsFull();
 */
```
