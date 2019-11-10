# leetcode 53 最大连续子数组和
## 题目地址
https://leetcode-cn.com/problems/maximum-subarray/

## 题目描述
```
题目：给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
## 思路


将求n个数的数组的最大子段和，转换为分别求出以第1个、第2个、...、第n个数字结尾的最大子段和，再找出n个结果中最大的，即为结果

### 解法1 - 动态规划
第i个状态 `dp[i]` 即为 `以第i个数字结尾`的最大子段和。由于以第i-1个数字结尾的最大子段和 (dp[i-1]) 与nums[i]相邻

若 dp[i-1] > 0 ，dp[i] = dp[i-1]+nums[i].
否则 dp[i] = nums[i]

边界值： 以第1个数字结尾的最大子段和 dp[0] = nums[0]

状态转移方程： dp[i] = max(dp[i-1]+nums[i],nums[i])


#### 空间复杂度O(n) 代码：

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0]*len(nums)
        dp[0] = nums[0]
        ans = dp[0]
        for i in range(1,len(nums)):
            dp[i] = max(dp[i-1]+nums[i],nums[i])
            if ans<dp[i]:
                ans = dp[i]
        return ans
```
#### 复杂度分析
时间复杂度 O(n)
空间复杂度 O(n)


**若要将空间复杂度降到O(1)**
只用一个变量记录当前最大子序列和，一个变量记录最终结果

1.定义当前最大连续子序列和sum=0，最大子序和ans=nums[0]
2.对数组进行遍历，对于nums[i]，存在两种情况：
- 若当前最大连续子序列和`sum>0`，说明sum对后续结果有着`正向增益`，即能使后续结果继续增大，则继续加和`sum = sum+nums[i]`。
- 若当前最大连续子序列和`sum<=0`，说明sum对后续结果没有增益或`负向增益`，`即若存在更大的加和，一定是从下一元素开始，加上sum，只会使结果更小。`因此，令`sum更新为nums[i]`。
更新最大子序和`ans=max(ans,sum)`始终保留最大结果。


### 解法2 - 动态规划 空间复杂度O(1) 代码：

``` python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        sum = 0
        ans = nums[0]
        n=len(nums)
        for i in range(n):
            if(sum>0):
                sum += nums[i]
            else:
                sum = nums[i]
            ans=max(ans,sum)    
        return ans

```
#### 复杂度分析
时间复杂度 O(n)
空间复杂度 O(1)

### 解法3 - 分治法
待更新






