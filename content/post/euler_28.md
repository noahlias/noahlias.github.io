---
title: "Number spiral diagonals"
date: 2022-11-30T11:02:03+08:00
draft: false
tags: ["Math", "Python"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 28**](https://projecteuler.net/problem=28)

## Description

Starting with the number 1 and moving to the right in a clockwise direction a 5 by 5 spiral is formed as follows:

21 22 23 24 25
20 7 8 9 10
19 6 1 2 11
18 5 4 3 12
17 16 15 14 13

It can be verified that the sum of the numbers on the diagonals is 101.

What is the sum of the numbers on the diagonals in a 1001 by 1001 spiral formed in the same way?

## Solution

从上面的图中找到每个斜边中的序列数字可以推导出下列公式:
$$\begin{array}{rll}
a_n &= (9, 25, 49, 81, 121, ...) &= 4n^2 + 4n + 1
\\ b_n &= (5, 17, 37, 65, 101, ...) &= 4n^2 + 1
\\ c_n &= (3, 13, 31, 57, 91, ...) &= 4n^2 - 2n+1
\\ d_n &= (7, 21,43, 73, 111, ...) &= 4n^2 + 2n + 1
\end{array}$$

汇总结果:
$$\begin{array}{rl}
s_n &= 1+\sum\limits_{i=1}^n (a_i + b_i + c_i + d_i)
\\ &= 1+\sum\limits_{i=1}^n (16i^2 + 4i + 4)
\\ &= 1+16\sum\limits_{i=1}^ni^2 + 4\sum\limits_{i=1}^ni + \sum\limits_{i=1}^n4
\\ &= 1+\frac{8}{3}n(n+1)(2n+1) + 2n(n+1) + 4n
\\ &= 1+\frac{2}{3}n(8n^2+15n+13)
\end{array}$$

```python
def solution(n):
    '''

    Solve the spiral diagonals
    >>> solution(1001)
    >>> 669171001
    '''
    n = (n-1)//2
    return 2 * n * (8 * n * n + 15 * n + 13) / 3 + 1
```

another solution

```python
n = 1001
print((n * (n * (4 * n + 3) + 8) - 9) / 6)
```

## Summary

- 找规律获取每个sequence的逻辑
- 注意中间点是1

## References

[xarg.org](https://www.xarg.org/puzzle/project-euler/problem-28/)

[Horner method](https://en.wikipedia.org/wiki/Horner%27s_method)
