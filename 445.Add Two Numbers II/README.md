You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

**Example:**
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
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
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
  var s1, s2 []int
  for ; l1 != nil; l1 = l1.Next {
    s1 = append(s1, l1.Val)
  }
  for ; l2 != nil; l2 = l2.Next {
    s2 = append(s2, l2.Val)
  }

  var carry int
  var ans *ListNode
  for len(s1) != 0 || len(s2) != 0 || carry != 0 {
    sum := carry
    if len(s1) != 0 {
      ei := len(s1) - 1
      sum += s1[ei]
      s1 = s1[:ei]
    }
    if len(s2) != 0 {
      ei := len(s2) - 1
      sum += s2[ei]
      s2 = s2[:ei]
    }

    carry = sum / 10
    node := &ListNode{Val: sum % 10}
    node.Next = ans
    ans = node
  }
  return ans
}
```
