Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**

You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
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
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) == 0 {
		return nil
	}

	root := &TreeNode{Val: preorder[0]}
	rootIdx := indexOf(inorder, preorder[0])

	leftInorder := inorder[:rootIdx]
	rightInorder := inorder[rootIdx + 1:]

	leftPreorder := preorder[1:1+len(leftInorder)]
	rightPreorder := preorder[1+len(leftInorder):]	

	root.Left = buildTree(leftPreorder, leftInorder)
	root.Right = buildTree(rightPreorder, rightInorder)

	return root
}

func indexOf(arr []int, val int) int {
	for idx, v := range arr {
		if v == val {
			return idx
		}
	}

	return -1
}
```
