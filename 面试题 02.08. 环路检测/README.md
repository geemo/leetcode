给定一个有环链表，实现一个算法返回环路的开头节点。
有环链表的定义：在链表中某个节点的next元素指向在它前面出现过的节点，则表明该链表存在环路。

**示例 1：**
```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```
**示例 2：**
```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```
**示例 3：**
```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
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
func detectCycle(head *ListNode) *ListNode {
  if head == nil || head.Next == nil {
    return nil
  }

  var slow, fast *ListNode = head, head.Next
  for fast != nil && fast.Next != nil {
    slow = slow.Next
    fast = fast.Next.Next
    if slow == fast {
      break
    }
  }
  if slow != fast {
    return nil
  }

  fast = head
  slow = slow.Next
  for slow != fast {
    slow = slow.Next
    fast = fast.Next
  }

  return slow
}
```
