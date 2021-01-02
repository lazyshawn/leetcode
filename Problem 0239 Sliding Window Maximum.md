## § Problem 239 Sliding Window Maximum
> [滑动窗口的最大值](
https://leetcode-cn.com/problems/sliding-window-maximum/)

给你一个整数数组 `nums` ，
有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。
你只可以看到在滑动窗口内的 `k` 个数字。
滑动窗口每次只向右移动一位。

**Q:** 返回滑动窗口中的最大值。


### Notes
1. 对于每个位置的元素，需要找到之前比其大的元素。
因此可以考虑使用 **单调递减队列** 。

1. **单调递减队列** 中储存了当前位置之前所有单调递减的元素位置。
滑动窗口移动到栈首的元素右侧之后，栈首元素将不会再出现在窗口内。
因此我们可以不断弹出栈首元素，直到栈首元素在窗口中为止。

1. 为了可以同时弹出队首和队尾的元素，我们需要使用双端队列。
满足这种单调性的双端队列一般称作 `「单调队列」` 。


**My Solution:** 
```cpp
class Solution {
public:
  std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
    std::deque<int> q; // 双端队列
    int n = nums.size();

    // 队列初始化
    for (int i=0; i<k; ++i) {
      while (!q.empty() && nums[q.back()]<nums[i]) {
        q.pop_back();
      }
      q.push_back(i);
    }
     
    // 开始弹出和推入
    std::vector<int> ans = {nums[q.front()]};
    for (int i=k; i<n; ++i) {
      // 处理队尾元素，保证单调递减
      while (!q.empty() && nums[q.back()]<nums[i]) {
        q.pop_back();
      }
      q.push_back(i);

      // 处理队首元素，保证队首元素在滑动窗口内
      while (q.front() <= i-k) {
        q.pop_front();
      }
      ans.push_back(nums[q.front()]);
    }
    return ans;
  }
};
```

