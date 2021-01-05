## § Problem 830 Positions of Large Groups
> [较大分组的位置](
https://leetcode-cn.com/problems/positions-of-large-groups/)

在一个由小写字母构成的字符串 `s` 中，包含由一些连续的相同字符所构成的分组。
例如，在字符串 s = "abbxxxxzyy" 中，
就含有 "a", "bb", "xxxx", "z" 和 "yy" 这样的一些分组。
分组可以用区间 `[start, end]` 表示，
其中 start 和 end 分别表示该分组的起始和终止位置的下标。
上例中的 "xxxx" 分组用区间表示为 [3,6] 。
我们称所有包含大于或等于三个连续字符的分组为 `较大分组` 。

**Q:** 找到每一个 `较大分组` 的区间，
按起始位置下标递增顺序排序后，返回结果。


### Notes
* 遍历数组找到满足要求的**连续区间**，容易想到用双指针。
本题只需要找连续的相同字符，可以只用索引下标 (单指针)，
即只判断当前字符和下一字符是否相同。

* **注意边界情况**。
本题的一般情况下，根据判断 `end` 位置和 `start` 位置的字符不同时更新返回数组，
此时end位置位于最后一个相同字符后。
为了保证判断条件相同，
处理最后一个分组时需要让 `end` 停在字符串末尾索引(n-1)之后，即停在 `n` 位置。



**My Solution:** 
```cpp
class Solution {
public:
  std::vector<std::vector<int>> largeGroupPositions(std::string s){
    int n = s.size();
    int start = 0, end = 1;
    std::vector<std::vector<int>> ans;

    while (end<n+1) {  // 遍历到n是为了处理最后一个分组
      if (s[start] != s[end] || end == n) {
        if (end+1 - start > 3) {
          ans.push_back({start, end});
        }
        start = end;
      }
      ++end;
    }
    return ans;
  }
};
```



