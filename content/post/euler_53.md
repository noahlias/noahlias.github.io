---
title: "Combinatoric selections"
date: 2022-12-02T19:41:03+08:00
draft: false
tags: ["Math","Python","Combinatoric"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 53**](https://projecteuler.net/problem=53)

## Description

There are exactly ten ways of selecting three from five, 12345:

123, 124, 125, 134, 135, 145, 234, 235, 245, and 345

In combinatorics, we use the notation,$\displaystyle \binom 5 3 = 10$
.

In general,$\displaystyle \binom n r = \dfrac{n!}{r!(n-r)!}$

, where $r \le n$, $n! = n \times (n-1) \times ... \times 3 \times 2 \times 1$, and $0! = 1$.

It is not until $n = 23$, that a value exceeds one-million:$\displaystyle \binom {23} {10} = 1144066$
.

How many, not necessarily distinct, values of $\displaystyle \binom n r$
 for $1 \le n \le 100$, are greater than one-million?

## Solution

```python

from math import factorial
ans = []
for i in range(1,101):
    for j in range(i+1):
        res = factorial(i)/ (factorial(j) * factorial(i-j))
        if res> 1000000:
            ans.append((i,i-j))
print(len(ans))

```

## Summary

这个题有很多的解法 最简单的就是暴力,其次可以优化，还可以用动态规划 二维数组解决
也可以一维 降低空间复杂度详情参考[53_overview](https://projecteuler.net/overview=053)

- **BruteForce**
- **Cache**
- **Math**
- **Pascal's Triangle**
- **DynamicProgramming**(`1-D Array` or `2-D Array`)
