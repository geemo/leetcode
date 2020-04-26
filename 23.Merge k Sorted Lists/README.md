Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**
```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
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
func mergeKLists(lists []*ListNode) *ListNode {
  n := len(lists)
  if n == 0 {
    return nil
  }

  return _mergeLists(lists, 0, n - 1)
}

func _mergeLists(lists []*ListNode, l, r int) *ListNode {
  if l == r {
    return lists[l]
  }

  m := l + (r - l) / 2
  left, right := _mergeLists(lists, l, m), _mergeLists(lists, m + 1, r)

  return merge(left, right)
}

func merge(l1, l2 *ListNode) *ListNode {
  dummy := &ListNode{}
  pre := dummy
  for l1 != nil || l2 != nil {
    if l1 == nil {
      pre.Next = &ListNode{Val: l2.Val}
      l2 = l2.Next
    } else if l2 == nil {
      pre.Next = &ListNode{Val: l1.Val}
      l1 = l1.Next
    } else if l1.Val <= l2.Val {
      pre.Next = &ListNode{Val: l1.Val}
      l1 = l1.Next
    } else {
      pre.Next = &ListNode{Val: l2.Val}
      l2 = l2.Next
    }
    pre = pre.Next
  }

  return dummy.Next
}
```
