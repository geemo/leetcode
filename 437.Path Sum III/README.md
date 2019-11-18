You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
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
func pathSum(root *TreeNode, sum int) int {
  if root == nil {
    return 0
  }
  return count(root, sum) + pathSum(root.Left, sum) + pathSum(root.Right, sum)
}

func count(root *TreeNode, sum int) int {
  if root == nil {
    return 0
  }

  if root.Val == sum {
    return 1 + count(root.Left, sum - root.Val) + count(root.Right, sum - root.Val)
  }
  return count(root.Left, sum - root.Val) + count(root.Right, sum - root.Val)
}
```
