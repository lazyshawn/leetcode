## § Problem 399 Evaluate Division
> [数值除法](
https://leetcode-cn.com/problems/evaluate-division/)

给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，
其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。
每个 Ai 或 Bi 是一个表示单个变量的字符串。

另有一些以数组 queries 表示的问题，
其中 queries[j] = [Cj, Dj] 表示第 j 个问题，
请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。
如果存在某个无法确定的答案，则用 -1.0 替代这个答案。
假设输入总是有效的，不会出现除数为0的情况，且不存在任何矛盾的结果。

**Q:** 返回 所有问题的答案 。


### Notes
* 本题旨在求变量之间的倍数关系，且该倍数关系具有传递性。
处理有传递性的问题可以使用 **并查集** ，
在并查集的 `合并` 与 `查询` 的操作中维护这些变量之间的倍数关系。

