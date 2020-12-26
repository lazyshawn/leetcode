## Problem 84 Largest Rectangle in Histogarm
> 柱状图中的最大矩形。

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。
每个柱子彼此相邻，且宽度为 1 。

**Q:** 求在该柱状图中，能够勾勒出来的矩形的最大面积。


### Notes 
1. 寻找某个元素两侧小于该值的最近元素(柱子)。
优先使用单调栈, 堆栈的过程中记录每个元素一侧柱子的位置。

1. **常数优化**。
从左到右遍历数组时，我们对位置 `i` 进行入栈操作时，
说明遍历到的位置 `j` 的高度小于等于 `h[i]`，
这就确定了位置 `i` 的左边界。
不难想到，与之对应的在位置 `i` 进行出栈操作时，可以确定它的右边界。
虽然此时的右边界不是严格小于，但不会对结果产生影响，
因为最终答案还是可以从连续相同高度的最右侧的位置处得到答案(矩形的最大面积)。


**My Solution:**
```cpp
#include <stack>
class Solution {
public:
  int largestRectangleArea (std::vector<int>& heights) {
    int n = heights.size(), maxArea = 0;
    std::vector<int> left(n), right(n);  // 分别记录左右侧的柱子位置

    std::stack<int> mono_stack;
    // 单调递增栈
    // 从左到右遍历
    for (int i=0; i<n; ++i) {
      // 当前值小于栈顶元素则弹栈
      while(!mono_stack.empty() && heights[i]<=heights[mono_stack.top()]){
        mono_stack.pop();
      }
      // 记录第i个值左侧第一个较小值
      left[i] = mono_stack.empty() ? -1 : mono_stack.top();
      // 当前值大于栈顶元素则堆栈
      mono_stack.push(i);
    }

    mono_stack = std::stack<int>();  // 清空堆栈
    // 从右到左遍历
    for (int i=n-1; i>=0; --i) {
      while(!mono_stack.empty() && heights[i]<=heights[mono_stack.top()]){
        mono_stack.pop();
      }
      right[i] = mono_stack.empty() ? n : mono_stack.top();
      mono_stack.push(i);
    }
    
    // 计算最大面积
    for (int i=0; i<n; ++i) {
      maxArea = std::max(maxArea, (right[i]-left[i]-1)*heights[i]);
    }
    return maxArea;
  }
};
```


