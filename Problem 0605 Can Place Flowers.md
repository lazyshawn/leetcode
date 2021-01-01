## § Problem 605 Can Place Flowers
> [605 种花问题](
https://leetcode-cn.com/problems/can-place-flowers/)

假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。
可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。
给定一个花坛 `flowerbed`
（表示为一个数组包含0和1，其中0表示没种植花，1表示种植了花），
和一个数 n 。

**Q:** 能否在不打破种植规则的情况下种入 n 朵花？
能则返回True，不能则返回False。


### Notes
* 最多能种植的花取决于连续的空地数量。
使用贪婪算法，每一块连续空地种满最大数量的花，最终的结果为最优解。

* 一般情况下，对于一块拥有 `n` 个空花盆的连续空地，
与其两端相邻的花盆种植了花，因此空地两端的花盆不能种花，
该空地最多可以种植 `(n-1)/2` 盆花。
花坛两边为特殊情况, 两端的空盆可以种植。
在花坛两边添加虚拟的空花盆 `0, flowerbed, 0, 1`，化为通常情况
(最后的 `1` 是为了让算法统计最后的一块空地)。


```cpp
class Solution {
public:
  bool canPlaceFlowers(std::vector<int>& flowerbed, int n) {
    // 在花坛两边插入虚拟地块
    flowerbed.insert(flowerbed.begin(),0);
    flowerbed.insert(flowerbed.end(),{0,1});

    int k = flowerbed.size();
    int count = 0, ans = 0;

    for (int i=0; i<k; ++i) {
      // 统计区间内 0 的个数
      count += flowerbed[i] ? 0 : 1;
      // 遇到已种植的花的地块
      if (flowerbed[i] == 1) {
        ans += (count-1) >> 1;
        count = 0;
      }
      // 满足条件提前退出
      if (ans + (count-1)/2 == n) return true;
    }
    return false;
  }
};
```


