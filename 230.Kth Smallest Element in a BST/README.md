Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

**Note:**

You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**
```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```
**Example 2:**
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```
**Follow up:**

What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

**Solution:**

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func kthSmallest(root *TreeNode, k int) int {
  var stack []*TreeNode

  for root != nil || len(stack) != 0 {
    for root != nil {
      stack = append(stack, root)
      root = root.Left
    }

    end := len(stack) - 1
    pop := stack[end]
    stack = stack[:end]
    k--
    if k == 0 {
      return pop.Val
    }
    if pop.Right != nil {
      root = pop.Right
    }
  }

  return -1
}
```
