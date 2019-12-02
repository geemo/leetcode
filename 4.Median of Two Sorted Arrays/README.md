There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume `nums1` and `nums2` cannot be both empty.

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

**Solutions:**

```golang
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
  len1, len2 := len(nums1), len(nums2)

  i, j := 0, 0
  isEven := (len1 + len2) % 2 == 0
  mid := (len1 + len2) / 2
  var arr []int
  for i < len1 && j < len2 {
    if nums1[i] < nums2[j] {
      arr = append(arr, nums1[i])
      i++
    } else {
      arr = append(arr, nums2[j])
      j++
    }
  }

  if i < len1 {
    arr = append(arr, nums1[i:]...)
  } else if j < len2 {
    arr = append(arr, nums2[j:]...)
  }

  if isEven {
    return float64(arr[mid - 1] + arr[mid]) / float64(2)
  }

  return float64(arr[mid])
}
```

```golang
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
  m, n := len(nums1), len(nums2)
  mid := (m + n) / 2
  left, right := -1, -1
  i1, i2 := 0, 0
  for i := 0; i <= mid; i++ {
    left = right
    if i1 < m && (i2 >= n || nums1[i1] < nums2[i2]) {
      right = nums1[i1]
      i1++
    } else {
      right = nums2[i2]
      i2++
    }
  }

  if (m + n) % 2 == 0 {
    return float64(left + right) / 2.0
  }
  return float64(right)
}
```
