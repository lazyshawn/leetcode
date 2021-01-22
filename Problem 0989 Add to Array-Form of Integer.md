## § Problem 989 Add to Array-Form of Integer
> [数组形式的整数加法](
https://leetcode-cn.com/problems/add-to-array-form-of-integer/)

对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。
例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。

**Q:** 给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。


### Notes
* 若先将数组形式的 X 化成整数再相加，则需要考虑溢出的问题。
若先将 K 化为数组形式再相加，则需要考虑数组对其的问题。

* 不转换类型进行逐位相加。
数组需要考虑越界的问题，所以优先处理。
整数 K 位数多于数组的部分单独考虑。


**My Solution:** 
```cpp
#include <algorithm>

class Solution {
  public:
    std::vector<int> addToArrayForm(std::vector<int>& A, int K) {
      std::vector<int> ans;
      int n = A.size();
      int sum = 0;

      for (int i=n-1; i>=0; --i) {
        sum = A[i] + K%10;
        K /= 10;
        ans.push_back(sum%10);

        // 处理进位
        if (sum >= 10) {
          K++;
        }
      }

      while(K) {
        ans.push_back(K%10);
        K /= 10;
      }
      reverse(ans.begin(), ans.end());
      return ans;
    }
};
```

