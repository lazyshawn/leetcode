## § Problem 959 Regions Cut By Slashes
> [由斜杠划分区域](
https://leetcode-cn.com/problems/regions-cut-by-slashes/)


在由 1 x 1 方格组成的 N x N 网格 grid 中，
每个 1 x 1 方块由 /、\ 或空格构成。
这些字符会将方块划分为一些共边的区域。
（请注意，反斜杠字符是转义的，因此 \ 用 "\\" 表示。）。

**Q:** 返回区域的数目。


### Notes
* 考虑组成方格的所有结点的连通性问题，
若斜杠将两个在同一连通分量中的结点连接，
则区域数加一。

* 结点的表示问题。
可以用表示为两维数组，
也可以表示为一维数组。


**My Solution:** 
```cpp
class Solution {
  public:
    // 查找
    int find(std::vector<int>& parents, int n) {
      return parents[n] == n ? n : find(parents, parents[n]);
    }

    // 合并
    void unify(std::vector<int>& parents, int i, int j) {
      parents[find(parents, i)] = find(parents, parents[j]);
    }

    int regionsBySlashes(std::vector<std::string>& grid) {
      // 结点个数
      int n = grid.size() + 1;
      std::vector<int> parents(n*n);
      int count = 1;

      // 初始化并查集
      for (int i=0; i<n*n; ++i) {
        parents[i] = i;
      }
      // 连接 NxN 网格四条边上的点
      for (int i=0; i<n; ++i) {
        parents[i] = 0;
        parents[n*n-n+i] = 0;
        parents[n*i] = 0;
        parents[n*(i+1)-1] = 0;
      }

      // 遍历每个方格
      for (int i=0; i<n-1; ++i) {
        for (int j=0; j<n-1; ++j) {
          if (grid[i][j] == '/'){
            // 判断 '/' 的连接情况
            count += find(parents, (i+1)*n+j) == find(parents, i*n+j+1) ? 1 : 0;
            unify(parents, (i+1)*n+j, i*n+j+1);
          } else if (grid[i][j] == '\\') {
            // 判断 '\\' 的连接情况
            count += find(parents, i*n+j) == find(parents, (i+1)*n+j+1) ? 1 : 0;
            unify(parents, i*n+j, (i+1)*n+j+1);
          }
        }
      }
      return count;
    }
};
```

