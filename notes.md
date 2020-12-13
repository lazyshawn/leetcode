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


## Problem 7. 整数反转
> 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
假设我们的环境只能存储32位的有符号整数，则其数值范围为$[-2^{31}, 2^{31}-1]$，
如果反转后整数溢出那么返回0。

### Notes
* **反转的基本操作**。
反转整数的方法可以与反转字符串进行类比。
重复弹出`x`的最后一位数字，并将它推入到`rev`后面。

* **反转时可能存在溢出问题**。
当`rev = rev*10 + pop`时可能会导致溢出。
假如`rev`为正数，由于`0<=pop<=9`那么(负数同理)：
  1. 如果 $temp=rev·10 + pop$ 导致溢出，那么一定有 $rev>=INT\_MAX/10$;
  1. 如果 $rev>INT\_MAX/10$，那么`temp`一定溢出；
  1. 如果 $rev==INT\_MAX/10$，那么$pop>INT\_MAX-rev$时会溢出(pop>7)。

* **可以使用同类型的存储位数更高的变量来判断是否溢出**。
如本题可以令`rev`为`long`类型，在返回时判断`rev`是否大于INT\_MAX(或小于INT\_MIN)，
即可判断是否溢出，根据要求返回即可。

**My solution:**
```cpp
class Solution {
public:
    int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > INT_MAX/10 || (rev == INT_MAX / 10 && pop > 7)) return 0;
            if (rev < INT_MIN/10 || (rev == INT_MIN / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
};
```


## Problem 8. 字符串转换整数
> 实现一个`atoi`函数，使其能够将字符串转换为整数。

首先，该函数会根据需要丢弃无用的开头字符，直到寻找到第一个非空的字符为止。
接下来的转化规则如下：

* 如果第一个非空字符为正或者负号时，
则将该符号与之后面尽可能多的连续数字字符组合起来，
形成一个有符号整数。
* 假如第一个非空字符是数字，
则直接将其与之后连续的数字字符组合起来，形成一个整数。
* 该字符串在有效的整数部分之后也可能会存在多余的字符，
那么这些字符可以被忽略，它们对函数不应该造成影响。

**注意：** 假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或
字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0 。

### Notes
* **涉及整数的运算需要注意溢出**。
对于溢出的处理方式通常可以转换为`INT_MAX`的逆操作。

* 确定有限状态机 (deterministic finite automaton, DFA)，也称自动机、有限状态机。

* 解决字符串模拟题的常规方法：有限状态机、正则表达式。

**My Solution:**
```cpp
class Solution {
public:
    int myAtoi(std::string s) {
      long long ans = 0;
      int n = s.size(), sym = 1;
      bool flag = false;  // 判断是否开始转换

      for (int i=0; i<n; ++i){
        if (!flag && s[i] == ' ') continue;  // 空字符
        else if (!flag && s[i] == '+'){   // 正号
          flag = true;
        }
        else if (!flag && s[i] == '-'){   // 负号
          sym = -1;
          flag = true;
        }
        else if (s[i] >= '0' && s[i] <= '9'){  // 数字
          flag = true;
          if (sym-1==0 && INT32_MAX - 10*ans < s[i] - '0') return INT32_MAX;
          if (sym+1==0 && INT32_MAX - 10*ans +1 < s[i] - '0') return INT32_MIN;
          ans = ans*10 + s[i]-'0';
        }
        else break;
      }

      return ans*sym;
    }
};
```

