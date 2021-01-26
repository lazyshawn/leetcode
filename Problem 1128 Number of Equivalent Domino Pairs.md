## § Problem 1128 Number of Equivalent Domino Pairs
> [等价多米诺骨牌对的数量](
https://leetcode-cn.com/problems/number-of-equivalent-domino-pairs/)


给你一个由一些多米诺骨牌组成的列表 dominoes。
如果其中某一张多米诺骨牌可以通过旋转 0 度或 180 度得到另一张多米诺骨牌，
我们就认为这两张牌是等价的。
形式上，dominoes[i] = [a, b] 和 dominoes[j] = [c, d] 
等价的前提是 a==c 且 b==d，或是 a==d 且 b==c。

**Q:** 找出满足 dominoes[i] 和 dominoes[j] 等价的骨牌对 (i, j) 的数量。


### Notes
* 注意到二元对中的元素均不大于 9，
因此我们可以将每一个二元对拼接成一个两位的正整数，
这样就无需使用哈希表统计元素数量，而直接使用长度为100的数组即可。


```cpp
class Solution {
  public:
    int numEquivDominoPairs(std::vector<std::vector<int>>& dominoes) {
      int ret=0;
      std::vector<int> num(100);

      for (auto& it : dominoes) {
        int val = it[0] < it[1] ? it[0] * 10 + it[1] : it[1] * 10 + it[0];
        ret += num[val];
        num[val]++;
      }
      return ret;
    }
};
```


