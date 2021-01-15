## § Problem 1202 Smallest String With Swaps
> [交换字符串中的元素](
https://leetcode-cn.com/problems/smallest-string-with-swaps/)

给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，
其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。
你可以 `任意多次交换` 在 pairs 中任意一对索引处的字符。

**Q:** 返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。


### Notes
* 交换关系具有传递性。a, b可交换、b, c可交换，则a, c可交换。
只关心是否连通，不关心连通路径。
因此可以使用并查集。


**My Solution:**
```cpp
#include <queue>
#inlcude <map>

class Solution {
public:
  /* *** 查询 *** */
  int find(std::vector<int> &parent, int i) {
    // 将叶节点直接指定根节点
    if (parent[i] == i) {
      return i;
    } else {
      parent[i] = find(parent, parent[i]);
      return parent[i];
    }
  }
  /* *** 合并 *** */
  void unify(std::vector<int> &parent, int i, int j) {
    // 把i的父节点指向j的父节点
    parent[find(parent, i)] = find(parent, j);
  }

  std::string smallestStringWithSwaps(std::string s, std::vector<std::vector<int>> &pairs) {
    int len = s.size();
    std::string ans(s);
    std::vector<int> parent(len);
    // 初始化并查集
    for (int i = 0; i < len; ++i) {
      parent[i] = i;
    }
    for (int i = 0; i < pairs.size(); ++i) {
      unify(parent, pairs[i][0], pairs[i][1]);
    }

    // 同属于相同集合的结点分别排序
    std::map<int, std::priority_queue<char, std::vector<char>, std::greater<char>>> charMap;
    for (int i = 0; i < len; ++i) {
      // 代表元为 parent[i] 的集合
      charMap[find(parent, i)].push(s[i]);
    }
    // 按索引依次获得最小字典序的元素，并在对应的优先队列弹栈
    for (int i = 0; i < len; ++i) {
      ans[i] = charMap[find(parent, i)].top();
      charMap[find(parent, i)].pop();
    }
    return ans;
  }
};
```

