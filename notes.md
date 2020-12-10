[toc]

## Problem 3. 无重复字符的最长子串
> 给定一个字符串，请你找出其中不含有重复字符的最长子串的长度。

### Keys
* **滑动窗口:**
逐步移动右指针，改变滑动窗口大小。不满足条件时移动左指针，改变滑动窗口位置。
以此遍历所有连续对象。

* **判断字符重复:**
  1. 哈希集合；
  1. 桶问题，每个字符分别对应0-255；

### Notes
* 循环过程中，判断条件和输出值的计算都应该在所有循环变量改变前或后。


## Problem 4. 寻找两个正序数组的中位数
> 给定两个大小为m和n的正序（从小到大）数组`nums1`和`nums2`。
请你找出并返回两个正序数组的中位数。
**进阶:** 你能设计一个时间复杂度为`O(log(m+n))`的算法解决此问题吗？

### Keys
* **二分法:** 如果对时间复杂度的要求有log，通常都需要用到二分查找。

* **奇偶性的虚操作:** 
对任意序列`a1, a2, ..., an`，可以在每个数前后均虚拟加入`#`，
将数列长度由`n`增长为`2n+1`，恒为奇数。
在这种情况下，**每个位置可以通过 / 2 得到元素在原来序列中的位置**。


## Problem 5. 最长回文子串
> 给定字符串`s`，找到`s`中的最长回文子串。你可以假设`s`中的最大长度为1000。

### Keys
* **动态规划:** 
  + 通过记录的子问题的解来得到原问题的解。
  + 状态转移方程。
  + 循环顺序。

**My solution:** 
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int maxlen = 0, begin = 0;  // 记录输出值
        int start=0, end=0;  // 回文子串中首尾元素的下标
        int n = s.size();
        int i = 0;   // 下标索引

        while(i < n) {
            start=i, end=i;
            while((i+1<n) && (s[i]==s[i+1])) {  // 找到连续相同字符，定位中心元素
                ++end;
                ++i;
            }
            while((start-1>=0) && (end+1<n) && (s[start-1]==s[end+1])) {  // 中心发散
                --start;
                ++end;
            }
            if(end-start+1 > maxlen){  // 更新解
                maxlen = end-start+1;
                begin = start;
            }
            ++i;
        }
        return s.substr(begin, maxlen);
    }
};
```


## Problem 6. Z字形变换
> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

### Keys
* **按行排序**。依次将所有元素放入对应的行数组。
* **按行访问**。直接推导每一行中元素在原数组中的索引下标。

### Notes
* **用算法的思维思考问题**。按行排序时，按照元素的下标依次放入每一行，
只有在第一行和最后一行时放置的方向才发生变化。
因此可以定义放置方向的标志符，在循环中，行数随标识符变化而增加或减小。

* **循环终止条件**。当无法或难以算出循环的终止条件时，
直接用判断时判断循环变量是否超出可行范围内即可。
如按行访问时，不用算出有几个Z形，直接将字符串索引下标增加一个周期，
当越界时终止即可。

* **循环条件相同的处理放在同一循环**。
如按行访问时，在同一层循环中先计算向下排序的元素(1$\sim$n行终止条件均为i+j\<n)，
再处理第2$\sim$n-1行，判断(cyclen-i+j\<n)。

* **循环体的检查**。是否完全遍历；是否为死循环。

**My solution:** 
```cpp
// 按行访问
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows==1) return s;  // 行数为1时cyclen为0，循环无法终止

        int n = s.size();
        int cyclen = 2*numRows-2;
        string ans;
        
        for (int i=0; i<numRows; ++i) {  // 逐行访问
            for (int j=0; i+j<n; j+=cyclen) {  // 周期循环
                ans += s[i+j];
                if (i!=0 && i!=numRows-1 && cyclen-i+j<n) {
                    ans += s[cyclen-i+j];
                }
            }
        }
        return ans;
    }
};
```
