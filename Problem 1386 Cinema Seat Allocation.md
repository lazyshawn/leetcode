## Problem 1386 Cinema Seat Allocation
> 安排电影院座位。

电影院的观影厅中有 n 行座位，行编号从 1 到 n ，
且每一行内总共有 10 个座位，列编号从 1 到 10 。
给你数组`reservedSeats`，包含所有已经被预约了的座位。
如`reservedSeats[i]=[3,8]`表示第 3 行第 8 个座位被预约了。
请你返回`最多能安排多少个 4 人家庭`。
4 人家庭要占据`同一行连续`的4个座位，或者安排过道两边各两个人。

| 1 2 3 | 4 5 6 7 | 8 9 10 |
| --- | --- | ---  |


### Notes
* 根据题意，对于每一个家庭只有三种安排座位的方法。即
  + 安排位置2, 3, 4, 5;
  + 安排位置4, 5, 6, 7;
  + 安排位置6, 7, 8, 9;

而且每一排最多坐两个家庭，这种情况固定的问题可以用位运算解决。
使用若干位的二进制数`bitmask`表示一排座位的预约情况，预约则对应位置1，否则置0。
对于情况一，如果`bitmask`中第0, 1, 2, 3 个二进制位均为0, 那么就可以安排座位，
即`bitmask` 和 $(00001111)_{2}$ 的按位或值保持为$(00001111)_{2}$不变。

* 只需将每一排的`bitmask`分别与$(11110000)\_2$, $(11000011)\_2$, $(11110000)\_2$
分别进行按位或运算，如果其中一个在运算后保持不变，那么这一排就可以安排一个家庭。
没有被预约的一排可以安排两个家庭，不需要记录。

* `unordered_map` 键值对的索引。
```cpp
/* unordered_map的迭代器是一个指针，指向这个元素，通过迭代器来取得它的值。*/
unordered_map<Key,T>::iterator it;
(*it).first;       // the key value (of type Key)
(*it).second;      // the mapped value (of type T)
(*it);             // the "element value" (of type pair<const Key,T>)

/* 它的键值分别是迭代器的first和second属性。 */
it->first;         // same as (*it).first   (the key value)
it->second;        // same as (*it).second  (the mapped value) 

/* Decomposition declarations of C++17 extension */
for (auto& [row, bitmask]: occupied) {
  if ((bitmask | right) == right) ++aans;
}
```


**My Solution:**
```cpp
class Solution {
public:
  int maxNumberOfFamilies(int n, std::vector<std::vector<int>>& reservedSeats) {
    // 位运算蒙版(从右往左依次对应第2-9个座位)
    int left = 0b11110000;
    int mid = 0b11000011;
    int right = 0b00001111;
    
    std::unordered_map<int, int> occupied;
    // 记录每一排的预定情况
    for (const std::vector<int>& seat : reservedSeats) {
      if (seat[1]>1 && seat[1]<10) {
        // 右移seat[1]-2位
        occupied[seat[0]] |= 1<<(seat[1]-2);
      }
    }

    // 整排都没预定的座位中共可安排的家庭数量
    int ans = (n-occupied.size())<<1;
    // 逐排遍历
    for (auto& row : occupied) {
      if (((row.second|left)==left) || ((row.second|mid)==mid) || 
          (row.second|right)==right) {
        ++ans;
      }
    }
    return ans;
  }
};
```




