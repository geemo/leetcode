Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

**Example:**

Given this linked list: `1->2->3->4->5`

For k = 2, you should return: `2->1->4->3->5`

For k = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.
d

**Solution:**

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
  a, b := head, head
  for i := 0; i < k; i++ {
    if b == nil {
      return head
    }
    b = b.Next
  }

  newHead := reverse(a, b)
  a.Next = reverseKGroup(b, k)
  return newHead
}

func reverse(a, b *ListNode) *ListNode {
  var pre, cur *ListNode = nil, a
  for cur != b {
    cur.Next, pre, cur = pre, cur, cur.Next
  }

  return pre
}
```
