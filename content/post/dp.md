---
title: "动态规划"
date: 2022-02-23T12:55:03+08:00
draft: true
tags: ["动态规划"]
categories: ["DP"]
---

# 动态规划


若确定给定问题具有重叠子问题和最优子结构，那么就可以使用动态规划求解。总体上看，求解可分为四步
1. **状态定义**:  构建问题最优解模型，包括问题最优解的定义、有哪些计算解的自变量;
2. **初始状态**:  确定基础子问题的解（即已知解），原问题和子问题的解都是以基础子问题的解为起始点，在迭代计算中得到的
3. **转移方程**: 确定原问题的解与子问题的解之间的关系是什么，以及使用何种选择规则从子问题最优解组合中选出原问题最优解
4. **返回值**: 确定应返回的问题的解是什么 即动态规划在何处停止迭代


## 斐波那契数列

- 状态定义： 设 dpdp 为一维数组，其中 dp[i]dp[i] 的值代表斐波那契数列的第 ii 个数字。
- 转移方程： dp[i + 1] = dp[i] + dp[i - 1]dp[i+1]=dp[i]+dp[i−1] ，即对应数列定义 f(n + 1) = f(n) + f(n - 1)f(n+1)=f(n)+f(n−1) ；
- 初始状态： dp[0] = 1dp[0]=1, dp[1] = 1dp[1]=1 ，即初始化前两个数字；
- 返回值： dp[n]dp[n] ，即斐波那契数列的第 n个数字。


## 丑数


```python
def nthUglyNumber(n: int) -> int:
    dp, a, b, c = [1] * n, 0, 0, 0
    for i in range(1, n):
        n2, n3, n5 = dp[a] * 2, dp[b] * 3, dp[c] * 5
        dp[i] = min(n2, n3, n5)
        if dp[i] == n2: a += 1
        if dp[i] == n3: b += 1
        if dp[i] == n5: c += 1
    return dp[-1]
```

### 解法

- **状态定义** : 设动态规划列表dp, dp[i] 代表第n+1 个丑数
- **转移方程**：
    1. 当索引a、b、c 满足以下条件, dp[i] 为三种情况的最小值;
    2. 每轮计算dp[i],需要更新索引 值 使其始终满足方程条件


- **初始状**：dp[0]=1, 第一个丑数为1
- **返回值**: dp[n-1], 即返回第n个丑数


### 零钱问题

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。
计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。
你可以认为每种硬币的数量是无限的.

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        dp = [0] + [float('inf')] * amount
        for i in range(1, amount+1):
            for coin in coins:
                if i >= coin:
                    dp[i] = min(dp[i], dp[i-coin] + 1)
        return dp[-1] if dp[-1] != float('inf') else -1

```

### 打家劫舍

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。


> 使用滚动数组降低空间复杂度 有点类似于斐波那契数列中的递归解法
```python

class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0

        size = len(nums)
        if size == 1:
            return nums[0]
        first, second = nums[0], max(nums[0], nums[1])
        for i in range(2, size):
            first, second = second, max(first + nums[i], second)
        return second
```

常见的动态规划解法

- **初始状态** 
- **状态转移方程**
> dp[i] = max(dp[i-2]+nums[i],dp[i-1])
- **边界条件**
> dp[0] = num[0] 只有一间房屋
> dp[1] = max(num[0],num[1]) 两间房屋选择最大的一间

```python

class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0

        size = len(nums)
        if size == 1:
            return nums[0]
        dp = [0] * size
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, size):
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
        return dp[size - 1]

```
