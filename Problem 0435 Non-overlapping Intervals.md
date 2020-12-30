## § Problem 435 Non-overlapping Intervals
> [无重叠区间](
https://leetcode-cn.com/problems/non-overlapping-intervals/)

给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。

* 可以认为区间的终点总是大于它的起点。
* 区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。

**Q:** 返回需要移除区间的最小数量。


### Notes
* **反向思维**。找到需要移除的区间最小数量等价于计算最多的无重复区间的数量。

* **贪心算法**。
将所有区间按右端点升序排列，依次选择 **右端点最小** 且与之前区间不重合的区间。
以第一个区间 `interval[0]` 为例，要选择最多的无重复区间，有以下情况：
  + 其余区间与 `interval[0]` 不重合，那么这第一个区间必然是最优解中的一个区间。
  + 有若干个区间与 `interval[0]` 重合，那么这些区间必然两两重叠，
  问题转化这若干区间中的一个与剩余区间构成的无重复区间个数, 
  而 `interval[0]` 与剩余区间不重叠，因此必然可以组成最优解之一。

* 贪心算法可以认为是动态规划算法的一个特例, 
其每一步做出一个局部最优的选择，最终的结果就是全局最优解 ( **贪心选择性质** )。
通常，一个算法问题使用暴力解法需要指数级时间，
如果能使用动态规划消除重叠子问题，就可以降到多项式级别的时间，
如果满足贪心选择性质，那么可以进一步降低时间复杂度，达到线性级别。

* C++11 中的匿名函数：
`[](const auto &u, const auto &v) { return u[1] < v[1]; }` 。

* `sort` 函数自定义排序规则时第三个参数为 **两个输入参数的函数指针** ，
最终算法按该函数返回真值的情况排序，
即排序后靠前的元素与靠后的元素按顺序传入该函数后返回值为真。


**My Solution:** 
```cpp
class Solution {
public:
  int eraseOverlapIntervals(std::vector<std::vector<int>> &intervals) {
    if(intervals.empty()) return 0;
    int n = intervals.size();

    // 自定义排序规则 + 匿名函数
    sort(intervals.begin(), intervals.end(),
        [](const auto &u, const auto &v){return u[1]<v[1];} );
    int right = intervals[0][1];
    int cnt = 1;

    for (int i=1; i<n; ++i) {
      if (intervals[i][0]>=right) {  // 接触不算重叠
        ++cnt;
        right = intervals[i][1];
      }
    }
    return n-cnt;
  }
};
```


