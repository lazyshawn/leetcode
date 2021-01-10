## § Problem 228 Summary Ranges
> [区间汇总](
https://leetcode-cn.com/problems/summary-ranges/)

给定一个无重复元素的有序整数数组 nums 。
返回 **恰好覆盖数组中所有数字** 的 **最小有序** 区间范围列表。
也就是说，nums 的每个元素都恰好被某个区间范围所覆盖，
并且不存在属于某个范围但不属于 nums 的数字 x 。
即求数组中所有连续区间。

**Q:** 所有区间范围。


### Notes
* 返回的数组变量每次写入的元素格式不同时，
先在循环中将每个元素赋值给临时变量 temp，
根据条件修改格式后在push\_back。

* 复合条件判断时，如果数组索引可能越位则将数组判断放在后边。
如 `end < n-1 && nums[end]+1 == nums[end+1]`。


**My Solution:** 
```cpp
class Solution {
public:
  std::vector<std::string> summaryRanges(std::vector<int>& nums) {
    std::vector<std::string> ret;
    int n = nums.size();
    int start=0, end=0;
    
    while (end < n) {
      // 寻找连续区间
      while (end < n-1 && nums[end]+1 == nums[end+1]) {
        ++end;
      }
      // 临时变量
      std::string temp = std::to_string(nums[start]);
      if (end != start) {
        temp.append("->" + std::to_string(nums[end]));
      }
      ret.push_back(temp);
      // 开始寻找下一个区间
      ++end;
      start = end;
    }
    return ret;
  }
};
```


