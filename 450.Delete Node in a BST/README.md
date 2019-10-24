Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
Note: Time complexity should be O(height of tree).

**Example:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7
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
func deleteNode(root *TreeNode, key int) *TreeNode {
  if root == nil {
    return root
  }

  if root.Val == key {
    if root.Left == nil {
      return root.Right
    }
    if root.Right == nil {
      return root.Left
    }
    minNode := getMinNode(root.Right)
    root.Right = deleteNode(root.Right, minNode.Val)
    minNode.Left, minNode.Right = root.Left, root.Right
    root.Left, root.Right = nil, nil
    return minNode
  } else if root.Val < key {
    root.Right = deleteNode(root.Right, key)
  } else {
    root.Left = deleteNode(root.Left, key)
  }

  return root
}

// get min node of right subtree
func getMinNode(node *TreeNode) *TreeNode {
  for node.Left != nil {
    node = node.Left
  }
  return node
}
```
