## § Problem 1232 Check If It Is a Straight Line
> [缀点成线](
https://leetcode-cn.com/problems/check-if-it-is-a-straight-line/)


在一个 XY 坐标系中有一些点，
我们用数组 coordinates 来分别记录它们的坐标，
其中 coordinates[i] = [x, y] 表示横坐标为 x、纵坐标为 y 的点。

**Q:** 判断这些点是否在该坐标系中属于同一条直线上，
是则返回 true，否则请返回 false。


### Notes
* 用直线的一般式 「(Ax+By+C=0)」 可以避免使用除法，不用考虑除零错误。
同时将坐标系原点移动到第一个点，则C=0。

* 用直线的两点式 「(y-y1)/(y-y2)=(x-x1)/(x-x2)」，
将除法转化为乘法后也可以不用考虑除零误差。


**My Solution:** 
```cpp
class Solution {
public:
  bool checkStraightLine(std::vector<std::vector<int>> &coordinates) {
    int deltaX = coordinates[0][0], deltaY = coordinates[0][1];
    int n = coordinates.size();
    // 坐标系原点移动到第一个点
    for (int i = 0; i < n; ++i) {
      coordinates[i][0] -= deltaX;
      coordinates[i][1] -= deltaY;
    }
    // 利用第二个点计算系数
    int A = coordinates[1][1], B = -coordinates[1][0];
    // 遍历数组
    for (int i = 2; i < n; ++i) {
      int x = coordinates[i][0], y = coordinates[i][1];
      if (A * x + B * y != 0) {
        return false;
      }
    }
    return true;
  }
};
```

