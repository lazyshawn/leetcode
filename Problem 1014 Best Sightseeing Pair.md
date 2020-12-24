## Problem 1014 Best Sightseeing Pair
> 最佳观光组合。

给定正整数数组 A，A[i] 表示第 i 个观光景点的评分，
并且两个景点 i 和 j 之间的距离为 j - i。
一对景点（i < j）组成的观光组合的得分为（A[i] + A[j] + i - j）：
景点的评分之和减去它们两者之间的距离。

**Q:** 返回一对观光景点能取得的最高分。


### Notes
1. `O(n^2)`能过的数据规模大概在1000左右，可以提前看一下题目给出的限制。

1. 将观光分数化为两部分：`A[i]+i`和`A[j]-j`。
那么对于观光点`j`，求其与之前的观光点组合的最大分数等价于
求`[0,j-1]`点的`A[i]+i`的最大值。

1. `[0,j-1]`点的`A[i]+i`的最大值只需要一个变量储存，且可以在遍历数组同时维护，
因此原问题只需要遍历一次数组，时间复杂度由`O(n^2)`降为`O(n)`。


**My Solution:**
```cpp
class Solution {
public:
  int maxScoreSightseeingPair(std::vector<int>& A) {
    int maxscore = 0, mx = A[0];
    for (int i=1; i<A.size(); ++i) {
      maxscore = std::max(maxscore, A[i]-i+mx);
      // 边遍历边维护
      mx = std::max(mx, A[i]+i);
    }
    return maxscore;
  }
};
```


