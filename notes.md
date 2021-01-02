[toc]

## § 线段树
* 线段树是一种二叉搜索树，与区间树相似，
它将一个区间划分成一些单元区间，每个单元区间对应线段树中的一个叶结点。
使用线段树可以**快速的查找某一个节点在若干条线段中出现的次数**，
时间复杂度为O(logN)。而未优化的空间复杂度为2N，
实际应用时一般还要开4N的数组以免越界，因此有时需要离散化让空间压缩。

* 只要能化成一些**连续点的修改和统计**问题，就可以用线段树来解决。

### 线段树的常见应用
* **区间内的绝对众数**。
  + Problem 57. 子数组中占绝大多数的元素。


### 线段树模板伪代码
```cpp
/* 节点数据结构体 */
struct node {
  typename val, cnt;   // 节点记录的值

  /* 节点的运算(线性加法) */
  node operator+ (const node& b) const {
    node ret;
    更新ret.val;
    更新ret.cnt;
    return ret;
  }
} t[65536];  // 节点结构体数组

/* 递归构造线段树 (对每个节点赋值) */
// R: 节点序号; l: 数组左端点索引; r:数组右端点索引;
void build (int R, int l, int r) {
  // 处理叶节点时
  if (r==l) {
    t[R].val = a[l];
    t[R].cnt = 1;
    return;
  }
  // 一般节点
  int mid = (r+l) >> 1;
  build(R<<1, l, mid);      // 左子节点
  build(R<<1|1, mid+1, r);  // 右子节点
  t[R] = t[R<<1] + t[R<<1|1];
};

/* 访问节点 (递归查找) */
// left: 查询区间的左端点索引; right: 查询区间的右端点索引;
node query(int R, int l, int r, int left, int right){
  // 匹配到区间左右端点
  if (l==left && r==right) {
    return t[R];
  }
  // 查询区间在子树中
  int mid = (l+r)>>1;
  if (right <= mid) {
    return query(R<<1, l, mid, left, right);
  } else if (left > mid) {
    return query(R<<1|1, mid+1, r, left, right);
  } else return query(R<<1, l, mid, left, mid)
    + query(R<<1|1, mid+1, r, mid+1, right);
}
```


## § 单调栈/队列
* **单调栈** 即单调递增或递减的栈。
这一数据结构本身并没有很重要的作用，
但是构建单调栈的算法可以快速地解决
**连续区间内的极值** 问题**。

* 构建单调栈的算法的核心思想是:
对于第 `j0`, `j1` 个元素满足`j0 < j1 < i`, `a[j0] > a[j1]`，
那么对于任意出现在它们之后的元素 `i (j1 < i)`，
`j1` 都不会是 `i` 左侧小于其值的元素。
那么对于单调递增栈，
在遍历数组的同时只需要维护一个最大值为当前元素`a[i]`的单调递增数组，
其余元素被弹出栈，因为对于之后的元素 `a[i]` 会优先满足小于的条件。
这样，在计算小于 `a[i]` 的元素时只需要比较之前元素构成的单调递增栈中的元素。
因此可以将查找的时间复杂度降为`O[N]`。

* 在遍历数组构建单调栈的同时可以使用一个新的数组
记录原数组中每个元素之前小于其值的第一个元素的位置。

* 为了可以同时弹出队首和队尾的元素，我们需要使用双端队列。
满足单调性的双端队列一般称作 `「单调队列」` 。


### 单调栈的常见应用
* **左右两侧最近的高度小于h[i]的柱子** 。
  + Problem 84. 柱状图中的最大矩形。


### 单调栈模板伪代码
```cpp
/* 单调递增栈 */
std::stack<int> mono_stack;  // 单调栈
std::deque<int> deque;  // 单调队列

// 从左到右遍历数组
for (int i=0; i<n; ++i) {
  // 当前值小于栈顶元素则弹栈
  while(!mono_stack.empty() && heights[i]<=heights[mono_stack.top()]){
    mono_stack.pop();
  }
  /* 操作1: 记录第i个值左侧第一个较小值的索引 */
  left[i] = mono_stack.empty() ? -1 : mono_stack.top();
  // 当前值大于栈顶元素则堆栈, 记录当前元素索引
  mono_stack.push(i);
  /* 操作2 */
  foo;
}
```


## § 动态规划
**动态规划** (Dynamic Programming, DP) 是求解决策过程中最优化的过程。
主要用于求解可划分为多阶段的动态过程的优化问题 (如以时间划分)。

### 动态规划的一般步骤
1. 确定模型 **状态** 。即对应与 **最优解** 的模型的值。
1. 确定 **自由变量** 。即决定模型状态的所有变量，用于构建动态规划数组 `dp` 的索引。
1. 推导 **状态转移方程** 。即当前问题与若干子问题的解之间的关系。
1. 设置 **边界条件** 。合法的边界条件根据实际问题算出，
不合法的边界条件可以设置为较大或较小的某常值 
(因为动态规划问题常用于解最值问题, 状态转移方程也使用极值函数)。

### 动态规划的常见应用
* **股票问题** 。
  + [188. 买卖股票的最佳时机 IV](
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)


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

* **循环顺序**。

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


## Problem 57. 子数组中占绝大多数的元素
> 实现一个`MajorityChecker`的类，它具有以下几个API：

* `MajorityChecker(int[] arr)` 会用给定的数组`arr`来构造一个`MajorityChecker`的实例。
* `int query(int left, int right, int threshold)` 中有这么几个参数：
  + `0 <= left <= right < arr.length` 表示数组`arr`的子数组的长度。
  + `2 * threshold > right - left + 1`， 
  也就是说阈值`threshold`始终比子序列长度的一半还要大。

每次查询`query(...)`会返回`arr[left], arr[left+1], ... , arr[right]` 中
至少出现`threshold` 次的元素，如果不存在就返回`-1`。

### Notes
* `2 * threshold > right - left + 1` 代表需要找的元素是给定数组中的众数，
且该众数是唯一的。

* 线段树是一种二叉搜索树，与区间树相似，
它将一个区间划分成一些单元区间，每个单元区间对应线段树中的一个叶结点。
使用线段树可以**快速的查找某一个节点在若干条线段中出现的次数**，
时间复杂度为O(logN)。而未优化的空间复杂度为2N，
实际应用时一般还要开4N的数组以免越界，因此有时需要离散化让空间压缩。

* 只要能化成一些**连续点的修改和统计**问题，就可以用线段树来解决。

* C++ 中赋值语句的返回值是所赋的值。

**My solution:**
```cpp
// 线段树模板
class SegmentTree {
  int n, a[20005];
  std::vector<int> s[20005];  // 容器数组

  // 节点数据结构体
  struct node {
    int val, cnt;   // 节点记录的值

    // 节点值的运算(线性加法)
    node operator+ (const node& b) const {
      node ret;
      if (val == b.val) {
        ret.val=val;
        ret.cnt = cnt + b.cnt;
      } else if (b.cnt > cnt) {
        ret.val = b.val;
        ret.cnt = b.cnt - cnt;
      } else {
        ret.val = val;
        ret.cnt = cnt - b.cnt;
      }
      return ret;
    }
  } t[65536];  // 节点结构体数组

  // 递归构造线段树(对每个节点赋值)
  void build (int R, int l, int r) {
    if (r==l) {
      t[R].val = a[l];
      t[R].cnt = 1;
      return;  // 达到叶节点时返回
    }
    int mid = (r+l) >> 1;
    build(R<<1, l, mid);      // 左子节点
    build(R<<1|1, mid+1, r);  // 右子节点
    t[R] = t[R<<1] + t[R<<1|1];
  };

  // 访问节点
  node query(int R, int l, int r, int left, int right){
    if (l==left && r==right) {
      return t[R];
    }
    int mid = (l+r)>>1;
    if (right <= mid) {
      return query(R<<1, l, mid, left, right);
    } else if (left > mid) {
      return query(R<<1|1, mid+1, r, left, right);
    } else return query(R<<1, l, mid, left, mid)
      + query(R<<1|1, mid+1, r, mid+1, right);
  }

public:
  SegmentTree(std::vector<int> &arr) {
    n = arr.size();
    int ans;
    // 赋值语句返回所赋的值
    // 赋值同时分别记录每个数的出现位置
    for (int i=0; i<n; ++i) {
      s[a[i]=arr[i]].push_back(i);
    }
    build(1,0,n-1);
  }

  int ask(int left, int right, int threshold) {
    int ans = query(1, 0, n-1, left, right).val;  // 绝对众数
    // 给定区间内数ans出现的次数
    // upper_bound, lower_bound 需要用到头文件<algorithm>
    if (upper_bound(s[ans].begin(), s[ans].end(), right) -
          lower_bound(s[ans].begin(), s[ans].end(), left) <
          threshold) ans = -1;
    return ans;
  }
};
```

### Ref
1. [线段树从零开始, 岩之痕, CSDN](
https://blog.csdn.net/zearot/article/details/52280189)


## Problem 1105
> 你把要摆放的书 books 都整理好，叠成一摞：
从上往下，第 i 本书的厚度为 books[i][0]，高度为 books[i][1]。
按顺序 将这些书摆放到总宽度为 shelf_width 的书架上。
返回书架整体可能的最小高度。

### Notes
* **动态规划问题**。
关键是理解当前问题与上一级问题的关联, 即状态转移方程。
  + 对于任意摆放情况，如果书架高度最小，那么去掉最后一层仍然是摆放剩余书籍的最优方式。
  因此只需要考虑最后一层的最大高度和放置剩余书籍的书架的最小高度。
  + 记放置前$i$本书需要的最小书架高度是$dp[i]$。
  设倒数第二层的最后一本书是第$j$本书，最后一层中最高的书高度为$h$，
  则$dp[i] = min(dp[j]+h)$。

* `for`循环中判断条件最好用循环次数，额外的退出条件写在循环内。
即常用`for(int i=0; i<n; ++i)`而非`for(int i=0; a[i]<fn; ++i)`。

**My Solution:**
```cpp
class Solution {
public:
  int minHeightShelves(std::vector<std::vector<int>>& books, int shelf_width) {
    int n = books.size();  // 书的数量
    std::vector<int> dp(n+1, INT32_MAX);
    dp[0] = 0;

    for (int i=1; i<n+1; ++i) { // 计算dp[i]
      int h = 0;
      int last_width = 0;
      for (int j=1; j<i+1; ++j) {
        // 最后一层书的总宽度和最大高度
        last_width += books[i-j][0];
        h = std::max(h, books[i-j][1]);
        if (last_width>shelf_width) break;

        dp[i] = std::min(dp[i], dp[i-j] + h);
      }
    }
    return dp[n];
  }
};
```

## Problem 1103
> 排排坐，分糖果。
我们买了一些糖果 `candies`，打算把它们分给排好队的 `n = num_people` 个小朋友。
给第一个小朋友 1 颗糖果，第二个小朋友 2 颗，依此类推，
直到给最后一个小朋友 n 颗糖果，再回到队伍起点，给第一个小朋友n+1颗。
返回一个长度为 `num_people`、元素之和为 `candies` 的数组，
以表示糖果的最终分发情况（即 `ans[i]` 表示第 `i` 个小朋友分到的糖果数）。

### Notes
* 圆周循环可以用求模运算记录，即用`i%n`表示第`i`次循环对应的元素索引。
* 求最值运算`min/max`有时可以代替`if`判断。

**My Solution:**
```cpp
class Solution {
public:
  std::vector<int> distributeCandies(int candies, int num_people) {
    std::vector<int> ans(num_people);
    int i = 0, tmp = 0;  // 循环次数，分配糖果数
    while(candies>0) {
      tmp = std::min(candies, i+1);
      ans[i%num_people] += tmp;
      candies -= tmp;
      ++i;
    }
    return ans;
  }
};
```


## Problem 434. Number of Segments in a String
> 字符串中的单词数。
统计字符串中的单词个数，这里单词指的是连续的非空格字符。

### Notes
* **Bool flag与零值比较。** Bool 值不直接与`true, false, 0, 1`进行比较，
直接用`if(flag)`或`if(!flag)`判断。

* **字符串的遍历。** `s[i]`

**My Solution:**
```cpp
class Solution {
public:
  int countSegments(std::string s){
    int cnt = 0;
    bool space = true;  // 记录上一个字符是否为空格

    for (int i=0; i<s.size(); ++i) {
      if (s[i]!=' ' && space){  // 空格后的字符非空格
        space = false;
        ++cnt;
      }
      if (s[i]==' '){
        space = true;
      }
    }

    return cnt;
  }
};
```


