Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

**Solutions:**

```golang
// recursively
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
  if head == nil || head.Next == nil {
    return head
  }

  subList := reverseList(head.Next)
  head.Next.Next = head
  head.Next = nil

  return subList
}
```

```golang
// iteratively
func reverseList(head *ListNode) *ListNode {
  if head == nil {
    return head
  }

  var curr, prev *ListNode = head, nil
  for curr != nil {
    next := curr.Next
    curr.Next = prev
    curr, prev = next, curr
  }

  return prev
}
```
