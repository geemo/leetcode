Shuffle a set of numbers without duplicates.

**Example:**
```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

**Solution:**

```golang
import (
  "math/rand"
  "time"
)

type Solution struct {
  nums []int
}


func Constructor(nums []int) Solution {
  rand.Seed(time.Now().UnixNano())
  return Solution {
    nums: nums,
  }
}


/** Resets the array to its original configuration and return it. */
func (this *Solution) Reset() []int {
  return this.nums
}


/** Returns a random shuffling of the array. */
func (this *Solution) Shuffle() []int {
  n := len(this.nums)
  nums := make([]int, n)
  copy(nums, this.nums)
  for i := n - 1; i > 0; i-- {
    ridx := rand.Intn(i + 1)
    nums[i], nums[ridx] = nums[ridx], nums[i]
  }
  return nums
}


/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.Reset();
 * param_2 := obj.Shuffle();
 */
```
