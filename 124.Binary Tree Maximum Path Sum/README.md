Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.

**Example 1:**

```
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

**Example 2:**

```
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

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
func maxPathSum(root *TreeNode) int {
  sum := -1 << 31
  var helper func(node *TreeNode) int
  helper = func(node *TreeNode) int {
    if node == nil {
      return 0
    }

    left, right := helper(node.Left), helper(node.Right)
    sum = max(sum, left + node.Val + right)
    return max(0, max(left, right) + node.Val)
  }

  helper(root)

  return sum
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}
```
