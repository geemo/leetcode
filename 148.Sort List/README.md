Sort a linked list in O(n log n) time using constant space complexity.

**Example 1:**
```
Input: 4->2->1->3
Output: 1->2->3->4
```

**Example 2:**
```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
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
func sortList(head *ListNode) *ListNode {
  if head == nil || head.Next == nil {
    return head
  }
  slow, fast := head, head.Next
  for fast != nil && fast.Next != nil {
    slow = slow.Next
    fast = fast.Next.Next
  }
  l2Head := slow.Next
  slow.Next = nil

  l1, l2 := sortList(head), sortList(l2Head)

  dummy := &ListNode{}
  pre := dummy
  for l1 != nil && l2 != nil {
    if l1.Val < l2.Val {
      pre.Next = l1
      l1 = l1.Next
    } else {
      pre.Next = l2
      l2 = l2.Next
    }
    pre = pre.Next
  }
  if l1 != nil {
    pre.Next = l1
  } else {
    pre.Next = l2
  }
  return dummy.Next
}
```
