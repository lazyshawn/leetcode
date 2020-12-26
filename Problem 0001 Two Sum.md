## Problem 1 Two Sum
> 两数之和。

**Q:** 
给定一个整数数组 `nums` 和一个目标值 `target`，
请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。


### Notes
1. 暴力枚举法需要遍历两次数组，时间复杂度为`O(n^2)`，
如果数据量过大会超时。

1. 使用哈希表，可以将寻找 **target - x** 的时间复杂度到从 `O(N)` 降低到 `O(1)`。

**My Solution:**
```cpp
class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
    for (int i = 0; i < nums.length; ++i) {
      if (hashtable.containsKey(target - nums[i])) {
        return new int[]{hashtable.get(target - nums[i]), i};
      }
      hashtable.put(nums[i], i);
    }
    return new int[0];
  }
}
```


