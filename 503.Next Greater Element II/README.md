Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

**Example 1:**

```
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number; 
The second 1's next greater number needs to search circularly, which is also 2.
```

**Solution:**

```golang
func nextGreaterElements(nums []int) []int {
  size := len(nums)
  if size == 0 {
    return nums
  }

  ans := make([]int, size)
  var stack []int
  for i := 2 * size - 1; i >= 0; i-- {
    idx := i % size
    for len(stack) != 0 && stack[0] <= nums[idx] {
      stack = stack[1:]
    }

    if len(stack) == 0 {
      ans[idx] = -1
    } else {
      ans[idx] = stack[0]
    }
    stack = append([]int{nums[idx]}, stack...)
  }
  
  return ans
}
```
