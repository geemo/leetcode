Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**Solution:**

```golang
func twoSum(nums []int, target int) []int {
	targetMap := make(map[int]int, len(nums))
	for idx, v := range nums {
		another := target - v
		if anotherIdx, ok := targetMap[another]; ok {
			return []int{idx, anotherIdx}
		}
		targetMap[v] = idx
	}
	return []int{}
}
```

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let obj = {};
    for (let i = 0; i < nums.length; i++) {
        let remaining = target - nums[i];
        if (obj[remaining] !== undefined) {
            return [i, obj[remaining]];
        }
        
        obj[nums[i]] = i;
    }
};
```
