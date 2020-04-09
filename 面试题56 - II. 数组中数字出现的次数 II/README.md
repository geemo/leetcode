在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

**示例 1：**
```
输入：nums = [3,4,3,3]
输出：4
```
**示例 2：**
```
输入：nums = [9,1,7,9,7,9,7]
输出：1
```

**限制：**

- 1 <= nums.length <= 10000
- 1 <= nums[i] < 2^31

**Solution:**

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
  let ones = 0, twos = 0, threes = 0;
  for (let num of nums) {
    twos |= ones & num;
    ones ^= num;
    threes = ones & twos;
    ones &= ~threes;
    twos &= ~threes;
  }

  return ones;
}
```
