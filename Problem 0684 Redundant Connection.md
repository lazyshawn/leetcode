## § Problem 684 Redundant Coonection
> [冗余连接](
https://leetcode-cn.com/problems/redundant-connection/)

在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。
附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。
每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

**Q:** 返回一条可以删去的边，使得结果图是一个有着N个节点的树。
如果有多个答案，则返回二维数组中最后出现的边。
答案边 [u, v] 应满足相同的格式 u < v。


### Notes
* 可以通过并查集寻找附加的边。
初始时，每个结点都属于不同的连通分量。
遍历每一条边，判断这条边连接的两个结点是否属于相同的连通分量。


**My Solution:** 
```cpp
class Solution {
public:
  /* *** 查询 *** */
  int find(std::vector<int>& parents, int i) {
    if (parents[i] == i) {
      return i;
    } else {
      parents[i] = find(parents, parents[i]);
      return parents[i];
    }
  }

  /* *** 合并 *** */
  void unify(std::vector<int>& parents, int i, int j) {
    parents[find(parents,i)] = find(parents,j);
  }

  /* *** 使用并查集 *** */
  std::vector<int> findRedundantConnection(std::vector<std::vector<int>>& edges) {
    int n = edges.size();
    std::vector<int> parents(n);
    // 初始化并查集
    for (int i=0; i<n; ++i) {
      parents[i] = i;
    }

    // 遍历每一条边，判断连接的结点是否属于相同的连通分量
    for (int i=0; i<n; ++i) {
      if (find(parents,edges[i][0]-1) != find(parents, edges[i][1]-1)) {
        unify(parents, edges[i][0]-1, edges[i][1]-1);
      } else {
        return edges[i];
      }
    }
    return edges[n-1];
  }
};
```

