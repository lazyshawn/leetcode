## § Problem 1319 Number of Operations to Make Network Connected
> [连通网络的操作次数](
https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)


用以太网线缆将 n 台计算机连接成一个网络，计算机的编号从 0 到 n-1。
线缆用 connections 表示，其中 connections[i] = [a, b] 连接了计算机 a 和 b。
网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。
给你这个计算机网络的初始布线 connections，
你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。

**Q:** 请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。 


### Notes
* 最少需要 n-1 根线缆, 即 `connections.size() >= n-1`。

* 等价于求连通分量的个数, 使用并查集可以解决。
初始化并查集后，连通分量是n个，每进行一次合并操作连通分量数减一。


**My Solution:** 
```cpp
class Solution {
  public:
    // 查询
    int find(std::vector<int>& parents, int n) {
      return parents[n] == n ? n : find(parents, parents[n]);
    }

    // 合并
    void unify(std::vector<int>& parents, int i, int j) {
      parents[find(parents, i)] = parents[find(parents, j)];
    }

    int makeConnected(int n, std::vector<std::vector<int>>& connections) {
      int len = connections.size();
      // 最少需要 n-1 条边
      if (len < n-1) return -1;

      // 初始化并查集
      std::vector<int> parents(n);
      for (int i=0; i<n; ++i) {
        parents[i] = i;
      }

      // 连接
      for (int i=0; i<len; ++i) {
        if (find(parents, connections[i][0]) != find(parents, connections[i][1])) {
          unify(parents, connections[i][0], connections[i][1]);
          // 连通分量数减一
          n--;
        }
      }
      return n-1;
    }
};
```

