Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

**Follow up:**
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

**Example:**
```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
```

**Solution:**

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
import (
  "math/rand"
  "time"
)

type Solution struct {
  head *ListNode
}


/** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
func Constructor(head *ListNode) Solution {
  rand.Seed(time.Now().UnixNano())
  return Solution {
    head: head,
  }
}


/** Returns a random node's value. */
func (this *Solution) GetRandom() int {
  cur := this.head
  var ans int = cur.Val
  for i := 1; cur != nil; i++ {
    if rand.Intn(i) == 0 {
      ans = cur.Val
    }
    cur = cur.Next
  }
  return ans
}


/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(head);
 * param_1 := obj.GetRandom();
 */
```
