## § Problem 1489 Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree
> [找到最小生成树里的关键边和伪关键边](
https://leetcode-cn.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)

给你一个 n 个点的带权无向连通图，
节点编号为 0 到 n-1 ，同时还有一个数组 edges ，
其中 edges[i] = [fromi, toi, weighti] 表示在 fromi 和 toi 节点之间有一条带权无向边。
最小生成树 (MST) 是给定图中边的一个子集，
它连接了所有节点且没有环，而且这些边的权值和最小。

如果从图中删去某条边，会导致最小生成树的权值和增加，
那么我们就说它是一条关键边。
伪关键边则是可能会出现在某些最小生成树中但不会出现在所有最小生成树中的边。

**Q:** 请你找到给定图中最小生成树的所有关键边和伪关键边。
请注意，你可以分别以任意顺序返回关键边的下标和伪关键边的下标。


