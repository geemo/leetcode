Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
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
func zigzagLevelOrder(root *TreeNode) [][]int {
  var ans [][]int
  if root == nil {
    return ans
  }

  curLevel := []*TreeNode{root}
  level := 0
  for len(curLevel) != 0 {
    var curLevelVal []int
    var nextLevel []*TreeNode

    for _, node := range curLevel {
      curLevelVal = append(curLevelVal, node.Val)
      if node.Left != nil {
        nextLevel = append(nextLevel, node.Left)
      }
      if node.Right != nil {
        nextLevel = append(nextLevel, node.Right)
      }
    }

    if level % 2 == 0 {
      ans = append(ans, curLevelVal)
    } else {
      ans = append(ans, reverse(curLevelVal))
    }
    level++
    curLevel = nextLevel
  }

  return ans
}

func reverse(arr []int) []int {
  l, r := 0, len(arr) - 1
  for l < r {
    arr[l], arr[r] = arr[r], arr[l]
    l++
    r--
  }
  return arr
}
```
