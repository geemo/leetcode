Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**
```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
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
func rightSideView(root *TreeNode) []int {
  var ans []int
  var dfs func(node *TreeNode, level int)
  dfs = func(node *TreeNode, level int) {
    if node == nil {
      return
    }

    if level >= len(ans) {
      ans = append(ans, node.Val)
    }
    level++
    dfs(node.Right, level)
    dfs(node.Left, level)
  }

  dfs(root, 0)
  return ans
}
```
