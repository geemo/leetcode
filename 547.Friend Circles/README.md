There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a N*N matrix M representing the friend relationship between students in the class. If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

**Example 1:**

```
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```

**Example 2:**

```
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```

**Note:**

- N is in range [1,200].
- M[i][i] = 1 for all students.
- If M[i][j] = 1, then M[j][i] = 1.

**Solution:**

```golang
type UFS struct {
  Roots []int
  Count int
}

func Constructor(n int) *UFS {
  ufs := &UFS{Roots: make([]int, n), Count: n}
  for i := 0; i < n; i++ {
    ufs.Roots[i] = i
  }

  return ufs
}

func (this *UFS) FindRoot(i int) int {
  root := i
  // find root
  for root != this.Roots[root] {
    root = this.Roots[root]
  }
  // compress path
  for i != this.Roots[i] {
    i, this.Roots[i] = this.Roots[i], root
  }

  return root
}

func (this *UFS) Union(p, q int) {
  pRoot, qRoot := this.FindRoot(p), this.FindRoot(q)
  if pRoot == qRoot {
    return
  }

  this.Roots[pRoot] = qRoot
  this.Count--
}

func findCircleNum(M [][]int) int {
  if len(M) == 0 || len(M[0]) == 0 {
    return 0
  }

  m, n := len(M), len(M[0])
  ufs := Constructor(m)
  for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
      if M[i][j] == 1 {
        ufs.Union(i, j)
      }
    }
  }

  return ufs.Count
}
```
