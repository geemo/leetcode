You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the left child node of the parent first if it exists.

**Example:**
```
Input: "4(2(3)(1))(6(5))"
Output: return the tree root node representing the following tree:

       4
     /   \
    2     6
   / \   / 
  3   1 5   
```
**Note:**

- There will only be '(', ')', '-' and '0' ~ '9' in the input string.
- An empty tree is represented by "" instead of "()".

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

import "strconv"

func str2tree(s string) *TreeNode {
  var stack []*TreeNode
  for i, n := 0, len(s); i < n; i++ {
    ch := s[i]
    if ch == ')' {
      stack = stack[:len(stack)-1]
    } else if ch >= '0' && ch <= '9' || ch == '-' {
      si := i
      for i < n - 1 && s[i+1] >= '0' && s[i+1] <= '9' {
        i++
      }

      num, _ := strconv.Atoi(s[si:i+1])
      root := &TreeNode{Val: num}

      if len(stack) != 0 {
        parent := stack[len(stack)-1]
        if parent.Left == nil {
          parent.Left = root
        } else {
          parent.Right = root
        }
      }

      stack = append(stack, root)
    }
  }

  if len(stack) == 0 {
    return nil
  }
  return stack[0]
}
```
