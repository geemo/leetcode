Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

**Note:** A leaf is a node with no children.

**Example:**
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
**Example 2:**
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

**Solution:**

dfs
```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func sumNumbers(root *TreeNode) int {
  return dfs(root, 0)
}

func dfs(root *TreeNode, sum int) int {
  if root == nil {
    return 0
  }
  if root.Left == nil && root.Right == nil {
    return sum * 10 + root.Val
  }

  sum = sum * 10 + root.Val
  return dfs(root.Left, sum) + dfs(root.Right, sum)
}
```

dfs stack
```golang
func sumNumbers(root *TreeNode) int {
  if root == nil {
    return 0
  }
  stack := []*TreeNode{root}
  var ans int
  for len(stack) != 0 {
    end := len(stack) - 1
    node := stack[end]
    stack = stack[:end]

    if node.Left == nil && node.Right == nil {
      ans += node.Val
      continue
    }

    if node.Right != nil {
      node.Right.Val += node.Val * 10
      stack = append(stack, node.Right)
    }
    if node.Left != nil {
      node.Left.Val += node.Val * 10
      stack = append(stack, node.Left)
    }
  }
  return ans
}
```
