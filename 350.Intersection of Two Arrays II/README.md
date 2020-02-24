
Given two arrays, write a function to compute their intersection.

**Example 1:**
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```
**Example 2:**
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```
**Note:**

- Each element in the result should appear as many times as it shows in both arrays.
- The  result can be in any order.
**Follow up:**

- What if the given array is already sorted? How would you optimize your algorithm?
- What if nums1's size is small compared to nums2's size? Which algorithm is better?
- What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

**Solution:**

```golang
import "sort"

func intersect(nums1 []int, nums2 []int) []int {
  var ans []int
  n1, n2 := len(nums1), len(nums2)
  if n1 == 0 || n2 == 0 {
    return ans
  }

  sort.Ints(nums1)
  sort.Ints(nums2)

  i1, i2 := 0, 0
  for i1 < n1 && i2 < n2 {
    if nums1[i1] == nums2[i2] {
      ans = append(ans, nums1[i1])
      i1++
      i2++
    } else if nums1[i1] < nums2[i2] {
      i1++
    } else {
      i2++
    }
  }

  return ans
}
```
