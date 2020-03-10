给定一棵二叉搜索树，请找出其中第k大的节点。

 
**示例 1:**
```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```
**示例 2:**
```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

**限制：**

- 1 ≤ k ≤ 二叉搜索树元素个数


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
func kthLargest(root *TreeNode, k int) int {
  var stack []*TreeNode
  for root != nil || len(stack) != 0 {
    for root != nil {
      stack = append(stack, root)
      root = root.Right
    }

    endIdx := len(stack) - 1
    pop := stack[endIdx]
    stack = stack[:endIdx]

    k--
    if k == 0 {
      return pop.Val
    }

    if pop.Left != nil {
      root = pop.Left
    }
  }

  return -1
}
```
