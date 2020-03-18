Given a complete binary tree, count the number of nodes.

**Note:**

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

**Example:**
```
Input: 
    1
   / \
  2   3
 / \  /
4  5 6

Output: 6
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
func countNodes(root *TreeNode) int {
  if root == nil {
    return 0
  }

  return 1 + countNodes(root.Left) + countNodes(root.Right)
}
```

```golang
func countNodes(root *TreeNode) int {
  var l, r *TreeNode = root, root
  var hl, hr int

  for l != nil {
    hl++
    l = l.Left
  }

  for r != nil {
    hr++
    r = r.Right
  }

  if hl == hr {
    return pow(2, hl) - 1
  }

  return countNodes(root.Left) + countNodes(root.Right) + 1
}

func pow(a, b int) int {
  ans := 1
  for ; b > 0; b-- {
    ans *= a
  }

  return ans
}
```
