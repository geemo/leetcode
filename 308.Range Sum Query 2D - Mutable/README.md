Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).


The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.

**Example:**
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```
**Note:**

- The matrix is only modifiable by the update function.
- You may assume the number of calls to update and sumRegion function is distributed evenly.
- You may assume that row1 ≤ row2 and col1 ≤ col2.

**Solution:**

```golang
type NumMatrix struct {
  root *SegTreeNode
  matrix [][]int
}


func Constructor(matrix [][]int) NumMatrix {
  if len(matrix) == 0 || len(matrix[0]) == 0 {
    return NumMatrix{}
  }

  return NumMatrix{
    root: buildTree(matrix, 0, 0, len(matrix) - 1, len(matrix[0]) - 1),
    matrix: matrix,
  }
}


func (this *NumMatrix) Update(row int, col int, val int)  {
  if this.root == nil {
    return
  }

  this.root.Update(row, col, val)
}


func (this *NumMatrix) SumRegion(row1 int, col1 int, row2 int, col2 int) int {
  if this.root == nil {
    return 0
  }

  return this.root.Query(row1, col1, row2, col2)
}

type SegTreeNode struct {
  ur, uc, lr, lc, sum int
  children [4]*SegTreeNode
}

func buildTree(matrix [][]int, ur, uc, lr, lc int) *SegTreeNode {
  if ur > lr || uc > lc {
    return nil
  }
  root := &SegTreeNode{ur: ur, uc: uc, lr: lr, lc: lc, sum: matrix[ur][uc]}
  if ur == lr && uc == lc {
    return root
  }

  mr, mc := ur + (lr - ur) / 2, uc + (lc - uc) / 2
  root.children[0] = buildTree(matrix, ur, uc, mr, mc)
  root.children[1] = buildTree(matrix, ur, mc + 1, mr, lc)
  root.children[2] = buildTree(matrix, mr + 1, uc, lr, mc)
  root.children[3] = buildTree(matrix, mr + 1, mc + 1, lr, lc)
  sum := 0
  for i := 0; i < 4; i++ {
    if root.children[i] != nil {
      sum += root.children[i].sum
    }
  }
  root.sum = sum

  return root
}

func (this *SegTreeNode) Update(r, c, val int) {
  if this.ur == this.lr && this.uc == this.lc {
    this.sum = val
    return
  }

  mr, mc := this.ur + (this.lr - this.ur) / 2, this.uc + (this.lc - this.uc) / 2
  if r <= mr && c <= mc {
    this.children[0].Update(r, c, val)
  } else if r <= mr && c > mc {
    this.children[1].Update(r, c, val)
  } else if r > mr && c <= mc {
    this.children[2].Update(r, c, val)
  } else {
    this.children[3].Update(r, c, val)
  }
  sum := 0
  for i := 0; i < 4; i++ {
    if this.children[i] != nil {
      sum += this.children[i].sum
    }
  }
  this.sum = sum
}

func (this *SegTreeNode) Query(ur, uc, lr, lc int) int {
  if this.ur == ur && this.uc == uc && this.lr == lr && this.lc == lc {
    return this.sum
  }

  mr, mc := this.ur + (this.lr - this.ur) / 2, this.uc + (this.lc - this.uc) / 2
  sum := 0
  if ur <= mr && uc <= mc {
    sum += this.children[0].Query(ur, uc, min(lr, mr), min(lc, mc))
  }
  if ur <= mr && lc > mc {
    sum += this.children[1].Query(ur, max(uc, mc + 1), min(lr, mr), lc)
  }
  if lr > mr && uc <= mc {
    sum += this.children[2].Query(max(ur, mr + 1), uc, lr, min(lc, mc))
  }
  if lr > mr && lc > mc {
    sum += this.children[3].Query(max(ur, mr + 1), max(uc, mc + 1), lr, lc)
  }

  return sum
}

func min(a, b int) int {
  if a < b {
    return a
  }
  return b
}

func max(a, b int) int {
  if a > b {
    return a
  }
  return b
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * obj := Constructor(matrix);
 * obj.Update(row,col,val);
 * param_2 := obj.SumRegion(row1,col1,row2,col2);
 */
```
