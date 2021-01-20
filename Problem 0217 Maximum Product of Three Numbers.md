##§ Problem 217 Maximum Product of Three Numbers
> [三个数的最大乘积](
https://leetcode-cn.com/problems/maximum-product-of-three-numbers/)

给定一个整型数组，在数组中找出由三个数组成的最大乘积，并输出这个乘积。


### Notes
* 三个数乘积最大的组合是三个最大的正数或者两个最小的负数和一个最大的正数。

* 排序后找出最大乘积即可。

* 不排序可以在遍历数组同时维护两个最小的数和三个最大的数。


**My Solution:** 
```cpp
class Solution {
public:
  int maximumProduct(vector<int>& nums) {
    // 最小的和第二小的
    int min1 = INT_MAX, min2 = INT_MAX;
    // 最大的、第二大的和第三大的
    int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;

    for (int x: nums) {
      // 维护最小的两个数
      if (x < min1) {
        min2 = min1;
        min1 = x;
      } else if (x < min2) {
        min2 = x;
      }

      // 维护最大的三个数
      if (x > max1) {
        max3 = max2;
        max2 = max1;
        max1 = x;
      } else if (x > max2) {
        max3 = max2;
        max2 = x;
      } else if (x > max3) {
        max3 = x;
      }
    }

    return max(min1 * min2 * max1, max1 * max2 * max3);
  }
};
```

