## § Problem 509 Fibonacci Number
> [斐波那契数](
https://leetcode-cn.com/problems/fibonacci-number/)

斐波那契数，通常用 F(n) 表示，形成的序列称为 斐波那契数列 。
该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：
```
F(0) = 0, F(1) = 1
F(n) = F(n-1) + F(n-2) , 其中n>1
```

**Q:** 给你 `n`， 请计算 `F(n)`。


### Notes
* 暴力解法可采用递归或者求通式。

* 斐波那契数有递推公式，可以用动态规划解题。
状态转移方程即为递推方程，边界条件为 `F(0), F(1)` 。

* 使用动态规划时，由于 `F(n)` 只和 `F(n-1), F(n)` 有关，
因此可以采用 `滚动数组思想` ，只需维护三个整数，
把空间复杂度优化为 `O(1)` 。


**My Solution:** 
```cpp
class Solution {
public:
  int fib(int n) {
    if (n<2) return n;

    // 滚动数组
    int p=0, q=1, r=1;
    for (int i=2; i<n; ++i) {
      p = q;
      q = r;
      r = p+q;
    }
    return r;
  }
};
```


