Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

**Solution:**

```golang
func nextPermutation(nums []int)  {
  n := len(nums)
  if n <= 1 {
    return
  }

  fi := n - 2
  for fi >= 0 {
    if nums[fi] < nums[fi + 1] {
      break
    }
    fi--
  }

  if fi == -1 {
    reverse(nums, 0, n - 1)
    return
  }

  si := n - 1
  for si >= 0 {
    if nums[si] > nums[fi] {
      break
    }
    si--
  }

  nums[fi], nums[si] = nums[si], nums[fi]
  reverse(nums, fi + 1, n - 1)
}

func reverse(nums []int, l, r int) {
  for l < r {
    nums[l], nums[r] = nums[r], nums[l]
    l++
    r--
  }
}
```
