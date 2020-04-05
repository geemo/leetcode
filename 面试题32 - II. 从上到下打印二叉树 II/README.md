从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

**例如:**
```
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```

**提示：**

- 节点总数 <= 1000

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
func levelOrder(root *TreeNode) [][]int {
  if root == nil {
    return nil
  }

  queue := []*TreeNode{root}
  var ans [][]int
  for len(queue) != 0 {
    size := len(queue)
    var level []int
    for ; size > 0; size-- {
      node := queue[0]
      queue = queue[1:]
      level = append(level, node.Val)

      if node.Left != nil {
        queue = append(queue, node.Left)
      }
      if node.Right != nil {
        queue = append(queue, node.Right)
      }
    }

    if len(level) != 0 {
      ans = append(ans, level)
    }
  }

  return ans
}
```
