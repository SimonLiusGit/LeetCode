# LeetCode121.买卖股票的最佳时机 
## 题目地址
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/submissions/

## 题目描述

```
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```

## 思路

### 解法一：暴力解法
由于限制只允许一次交易，那么一个最直观的解法就是对每一天，都依次遍历它之后的所有可能卖出的情况，记录最大值，最后进行比较得出最终结果。很显然这是一个二重循环，时间复杂度为O(n^2)，空间复杂度：O(1)。

换句话说，这也就是将所有可能的买卖情况都穷举出来，然后找最大值。

### 解法二：动态规划

由于我们是想获取到最大的利润，我们的策略应该是低点买入，高点卖出。

采用动态规划的思想
如果我们用`dp[i]`代表从`第1天到第i天进行一笔交易的最大收益`，我们设法找到dp[i-1]和dp[i]的关系，假想已经已知dp[i-1]也就是从第1天到第i-1天的最大收益，那么如果第i天的价格减去前i-1天的最低价格 比 dp[i-1]大，dp[i]就等于prices[i]-之前最小值。否则就还等于dp[i-1]。
这样，`前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}`
也就是 `dp[i] = max(dp[i-1],prices[i]-min)`,用min表示买入时的最低价格



## 代码


Python Code:

```python
class Solution:
    #动态规划 : 前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: return 0
        dp = [0]*(len(prices)+1)
        min = prices[0]
        for i in range(len(prices)):
            if min > prices[i]:
                min = prices[i]
            dp[i] = max(dp[i-1],prices[i]-min)
        return dp[len(prices)-1]
```

## 复杂度分析
- 时间复杂度：O(N) 只需要遍历一次
- 空间复杂度：O(N)

## 优化
在第二种解法上面，因为本题只需要返回最后i = n时的最大利润，中间结果是不需要保存的，所以可以不用dp数组记录中间答案，只需保留最大值即可。

### 代码
```python
class Solution:
    #动态规划 : 前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: return 0
        maxP = 0
        minP = prices[0]
        for i in range(len(prices)):
            minP = min(minP,prices[i])
            maxP = max(maxP,prices[i]-minP)
        return maxP
```

- 时间复杂度：O(N) 只需要遍历一次
- 空间复杂度：O(1),只用到两个变量
