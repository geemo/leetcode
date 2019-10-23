Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

 

**Example:**

![bst](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

**Note:**

- `next()` and `hasNext()` should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
- You may assume that `next()` call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.

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
type BSTIterator struct {
    data []int
}


func Constructor(root *TreeNode) BSTIterator {
  var data []int
  var inorderTraversal func(node *TreeNode)
  inorderTraversal = func(node *TreeNode) {
    if node == nil {
      return
    }

    inorderTraversal(node.Left)
    data = append(data, node.Val)
    inorderTraversal(node.Right)
  }
  inorderTraversal(root)
  return BSTIterator{data: data}
}


/** @return the next smallest number */
func (this *BSTIterator) Next() int {
  val := this.data[0]
  this.data = this.data[1:]
  return val
}


/** @return whether we have a next smallest number */
func (this *BSTIterator) HasNext() bool {
  return len(this.data) != 0
}


/**
 * Your BSTIterator object will be instantiated and called as such:
 * obj := Constructor(root);
 * param_1 := obj.Next();
 * param_2 := obj.HasNext();
 */
```
