## § Problem 1046 Last Stone Weight
> [最后一块石头的重量](
https://leetcode-cn.com/problems/last-stone-weight/)

有一堆石头，每块石头的重量都是正整数。
每一回合，从中选出两块 最重的 石头，然后将它们一起粉碎。
假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

* 如果 x == y，那么两块石头都会被完全粉碎；
* 如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。

**Q:** 最后，最多只会剩下一块石头。
返回此石头的重量。如果没有石头剩下，就返回 0。

### Notes
* **最大堆** 和 **优先队列** , 自动排序。
* **多次排序** 时可以考虑筛选不需要排序的数组元素。


**My Solution:** 
```cpp
class Solution {
public:
  int lastStoneWeight(std::vector<int> stones) {
    int k = stones.size();  // 标记需要排序的数组位置

    while(k>1) {
      std::sort(stones.begin(),stones.begin()+k);
      // 如果最大的两个石头相等，则指针前移两位
      if (stones[k-1]-stones[k-2] == 0) {
        k -= 2;
      } else {
      // 最大的两个石头不相等，则将差值赋予靠前者, 且指针前移一位
        stones[k-2] = stones[k-1]-stones[k-2];
        --k;
      }
    }
    return k ? stones[0] : 0;
  }
};
```

