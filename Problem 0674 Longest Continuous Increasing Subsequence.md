## § Problem 674 Longest Continuous Increasing Subsequence
> [最长连续递增序列](
https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)


给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，
如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，
那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。


### Notes
* 连续数组的问题可以选择使用指针作为连续区间的端点标记，
或者逐位计算是否满足条件，以累加或重置区间长度。


**My Solution:** 
```cpp
class Solution {
public:
  int findLengthOfLCIS(vector<int>& nums) {
    int ans = 0;
    int n = nums.size();
    int start = 0;
    for (int i = 0; i < n; i++) {
      // 重置区间起点
      if (i > 0 && nums[i] <= nums[i - 1]) {
        start = i;
      }
      // 计算区间长度
      ans = max(ans, i - start + 1);
    }
    return ans;
  }
};
```

