A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).

![pic](https://assets.leetcode.com/uploads/2018/10/22/skyline2.png)
 
The geometric information of each building is represented by a triplet of integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as: [ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .

The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ].

**Notes:**

- The number of buildings in any input list is guaranteed to be in the range [0, 10000].
- The input list is already sorted in ascending order by the left x position Li.
- The output list must be sorted by the x position.
- There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...[2 3], [4 5], [7 5], [11 5], [12 7]...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...[2 3], [4 5], [12 7], ...]

**Solution:**

```golang
import "container/heap"
import "sort"
func getSkyline(buildings [][]int) [][]int {
    var ret [][]int
    h := &NodeHeap{}
    heap.Init(h)
    var edgePoints []Node
    for i := 0; i < len(buildings); i++ {
        edgePoints = append(edgePoints, Node{buildings[i][0], buildings[i][2], i, true, 0})
        edgePoints = append(edgePoints, Node{buildings[i][1], buildings[i][2], i, false, 0})
    }
    sort.Slice(edgePoints, func(i, j int) bool { 
        if edgePoints[i].x != edgePoints[j].x {
            return edgePoints[i].x < edgePoints[j].x
        } else {
            return edgePoints[i].y < edgePoints[j].y
        }
    }) // O(n*logn)
    heap.Push(h, &Node{x:0, y:0}) // guard
    buildingsNodeMap := make([]*Node, len(buildings))
    for i := 0; i < len(edgePoints); i++ {
        p := edgePoints[i]
        if p.left {
            p.pos = h.Len()
            heap.Push(h, &p)
            buildingsNodeMap[p.idx] = &p
        } else {
            heap.Remove(h, buildingsNodeMap[p.idx].pos)
        }
        if i == len(edgePoints) - 1 || p.x != edgePoints[i+1].x { 
            currMaxHeight := (*h)[0].y
            if len(ret) == 0 || currMaxHeight != ret[len(ret)-1][1] {
                ret = append(ret, []int{p.x, currMaxHeight})
            }
        } 
    }
    return ret
}

type Node struct {
    x, y, idx int
    left bool
    pos int
}
type NodeHeap []*Node
func (h NodeHeap) Swap (i, j int) { 
    h[i], h[j] = h[j], h[i] 
    h[i].pos, h[j].pos = i, j
}
func (h NodeHeap) Len() int { return len(h) }
func (h NodeHeap) Less(i, j int) bool { return h[i].y > h[j].y } // max-heap based on y
func (h *NodeHeap) Push(v interface{}) { *h = append(*h, v.(*Node)) }
func (h *NodeHeap) Pop() interface{} {
    ret := (*h)[len(*h)-1]
    *h = (*h)[:len(*h)-1]
    return ret
}
```
