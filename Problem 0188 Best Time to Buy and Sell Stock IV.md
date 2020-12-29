## § Problem 188 Best Time to Buy and Sell Stock IV
> [188. 买卖股票的最佳时机IV](
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

给定一个整数数组 `prices` ，
它的第 `i` 个元素 `prices[i]` 是一支给定的股票在第 `i` 天的价格。
你最多可以完成 `k` 笔交易。
且你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**Q:** 设计一个算法来计算你所能获取的最大利润。

### Notes
* 分别完成一次买入和卖出算一次交易。
* 每天的状态可以由之前天数的状态推出，因此考虑 **动态规划** 。
* 动态规划问题需要考虑的几点：
  + 状态。本题中的最大收益。
  + 自由变量。决定状态的变量，如本题的天数 `i`, 最大交易次数 `k`, 当天持有股票状态。
  + 状态转移方程。


**My Solution:** 
```cpp
class Solution {
public:
  int maxProfit(int k, std::vector<int>& prices) {
    if (prices.empty()) {
      return 0;
    }

    int n = prices.size();
    k = std::min(k,n/2);

    std::vector<std::vector<int>> buy(n,std::vector<int>(k+1));
    std::vector<std::vector<int>> sell(n,std::vector<int>(k+1));
    
    /* 边界条件 */
    buy[0][0] = -prices[0];
    sell[0][0] = 0;
    for (int i=1; i<k+1; ++i) {
      // 不能直接用INT_MIN，因为可能需要作被减数
      buy[0][i] = sell[0][i] = INT32_MIN/2;  
    }

    /* 状态转移 */
    for (int i=1; i<n; ++i) {
      buy[i][0] = std::max(sell[i-1][0]-prices[i], buy[i-1][0]);
      for (int j=1; j<k+1; ++j) {
        buy[i][j] = std::max(sell[i-1][j]-prices[i], buy[i-1][j]);
        sell[i][j] = std::max(buy[i-1][j-1]+prices[i], sell[i-1][j]);
      }
    }

    return *std::max_element(sell[n-1].begin(), sell[n-1].end());
  }
};
```


### References
1. [股票问题系列通解(转载翻译)](
https://leetcode-cn.com/circle/article/qiAgHn/)

