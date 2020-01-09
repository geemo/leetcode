Given a singly linked list, determine if it is a palindrome.

**Example 1:**
```
Input: 1->2
Output: false
```
**Example 2:**
```
Input: 1->2->2->1
Output: true
```
**Follow up:**

Could you do it in O(n) time and O(1) space?

**Solution:**

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func isPalindrome(head *ListNode) bool {
  if head == nil || head.Next == nil {
    return true
  }
  var slow, fast *ListNode = head, head.Next
  var pre, prepre *ListNode
  for fast != nil && fast.Next != nil {
    pre = slow
    slow = slow.Next
    fast = fast.Next.Next

    pre.Next = prepre
    prepre = pre
  }

  var l1, l2 *ListNode = slow, slow.Next
  if fast == nil {
    l1 = pre
  } else {
    slow.Next = pre
  }
  
  for l1 != nil {
    if l1.Val != l2.Val {
      return false
    }
    l1, l2 = l1.Next, l2.Next
  }

  return l1 == l2
}
```
