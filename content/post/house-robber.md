---
title: "house-robber"
date: 2022-12-14T09:55:03+08:00
draft: false
tags: ["每日一题","LeetCode_Medium","DynamicProgramming"]
categories: ["DailyChallenge"]
---

[198.house-robber](https://leetcode.com/problems/house-robber/)

## Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.

大意就是你是一个专业的抢劫犯,街道上每个房子有着固定的金钱,唯一限制你抢劫的是 如果在同一晚上 你抢劫了相邻的房子 会自动联系警察 

现在给出一个整数数组 代表每个房子拥有的金钱 然后返回没有警报的情况下 你能在今晚抢劫到的
最大金钱

## Solution

```python

class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums)==1: return nums[0]
        n = len(nums)
        dp = [0]*(n)
        dp[0] = nums[0]
        dp[1] = max(nums[0],nums[1])
        for i in range(2, n):
            dp[i] =  max(dp[i-2]+nums[i],dp[i-1])
        return dp[-1]
```

下面使用**滚动数组**降低空间复杂度
上面的转换到下面其实就是数学的公式推导 到迭代

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums)==1: return nums[0]
        n = len(nums)
        a, b = nums[0], max(nums[0],nums[1])
        for i in range(2, n):
            a , b =  b ,max(a+nums[i],b)
        return b
```

## Summary

简单总结下这个题动态规划的解法:

- 初始状态 当只有1个房子和2个房子的时候结果如何
- 状态转移方程: $dp[i] = max(dp[i-2]+nums[i],dp[i-1])$

如何理解这个方程,首先可以思考子问题是什么以及问题结果和限制条件

- 子问题是最大的抢劫金钱
- 限制条件是不能相邻的房子