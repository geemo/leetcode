Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

**Example:**
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
**Follow up:**

- A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
- Could you come up with a one-pass algorithm using only constant space?

**Solution:**

```golang
func sortColors(nums []int) {
  quickSort(nums, 0, len(nums) - 1)
}

func quickSort(nums []int, l, r int) {
  if l >= r {
    return
  }

  x, i, j := nums[l], l - 1, r + 1
  for i < j {
    i++
    for nums[i] < x {
      i++
    }
    j--
    for nums[j] > x {
      j--
    }
    if i < j {
      nums[i], nums[j] = nums[j], nums[i]
    }
  }

  quickSort(nums, l, j)
  quickSort(nums, j + 1, r)
}
```
