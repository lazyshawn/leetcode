## Problem 455 Assign Cookies
> 分发饼干。

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。
饼干被分成了为`j`块，每块的尺寸分别为`s[j]`，每个孩子最多只能给一块饼干。
对每个孩子 i，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；
并且每块饼干 `j` ，都有一个尺寸 `s[j]` 。
如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。

**Q:** 你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。


### Notes
1. 为尽可能满足更多的孩子，从贪心的角度考虑，
应该按照孩子的胃口从小到大依次满足每个孩子，
且对于每个孩子，应该选择满足该孩子胃口的尺寸最小的饼干。


**My Solution:**
```cpp
class Solution {
public:
  int findContentChildren(std::vector<int>& g, std::vector<int>& s) {
    int count = 0;
    int numOfChildren = g.size(), numOfCookies = s.size();

    sort(g.begin(), g.end());
    sort(s.begin(), s.end());

    int j = 0;
    for (int i=0; i<numOfChildren; ++i) {  // 遍历所有小朋友
      while (j<numOfCookies) {  // 为第 i 个小朋友分配饼干
        if (s[j] >= g[i]) {
          ++count;
          ++j;
          break;
        }
        ++j;
      }
    }
    return count;
  }
};
```


