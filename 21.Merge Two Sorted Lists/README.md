Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

**Solutions:**

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
// recursively
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
  if l1 == nil {
    return l2
  } else if l2 == nil {
    return l1
  }

  if l1.Val < l2.Val {
    l1.Next = mergeTwoLists(l1.Next, l2)
    return l1
  }
  l2.Next = mergeTwoLists(l1, l2.Next)
  return l2
}
```

```golang
// iteratively
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
  if l1 == nil {
    return l2
  } else if l2 == nil {
    return l1
  }

  dummy := &ListNode{}
  head := dummy
  for l1 != nil && l2 != nil {
    if l1.Val < l2.Val {
      head.Next = l1
      l1 = l1.Next
    } else {
      head.Next = l2
      l2 = l2.Next
    }
    head = head.Next
  }
  
  if l1 == nil {
    head.Next = l2
  } else {
    head.Next = l1
  }
  
  return dummy.Next
}
```
