Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points `(i, j, k)` such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

**Example:**

```
Input:
[[0,0],[1,0],[2,0]]

Output:
2

Explanation:
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
```

**Solution:**

```golang
func numberOfBoomerangs(points [][]int) int {
    pLen := len(points)
    count := 0
    for i := 0; i < pLen; i++ {
        m := make(map[int]int)
        for j := 0; j < pLen; j++ {
            if i != j {
                _dis := dis(points[i], points[j])
                if _, ok := m[_dis]; ok {
                    m[_dis] += 1
                } else {
                    m[_dis] = 1
                }
            }
        }

        for _, v := range m {
            count += v * (v - 1)
        }
    }

    return count
}

func dis(a, b []int) int {
    disX := a[0] - b[0]
    disY := a[1] - b[1]

    return disX * disX + disY * disY
}
```
