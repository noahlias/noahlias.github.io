---
title: "Largest product in a grid"
date: 2022-11-23T10:48:03+08:00
draft: false
tags: ["Math","Python","Matrix"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 11**](https://projecteuler.net/problem=11)

## Description

In the 20×20 grid below, four numbers along a diagonal line have been marked in red.

The product of these numbers is 26 × 63 × 78 × 14 = 1788696.

What is the greatest product of four adjacent numbers in the same direction (up, down, left, right, or diagonally) in the 20×20 grid?

## Solution

![problem11](/plot/problem11.svg)

```python
def solution(data):
    ''' 
    
    Parse the data into the matrix
    and return the solution.
    >>> solution(data)
    >>> 70600674
    
    
    '''
    matrix = [[0] * 20 for _ in range(20)]
    for index, value in enumerate(data.split("\n")):
        matrix[index] = list(map(int, value.split(" ")))
    ans = 0
    for i in range(17):
        for j in range(17):
            a = matrix[i][j] * matrix[i + 1][j] * matrix[i + 2][j] * matrix[i + 3][j]
            b = matrix[i][j + 1] * matrix[i][j + 2] * matrix[i][j + 2] * matrix[i][j + 3]
            c = (
                matrix[i][j]
                * matrix[i + 1][j + 1]
                * matrix[i + 2][j + 2]
                * matrix[i + 3][j + 3]
            )
            f = (
                matrix[i + 3][j]
                * matrix[i][j + 3]
                * matrix[i + 1][j + 2]
                * matrix[i + 2][j + 1]
            )
            max_four_dimenson_matrix = max(a, b, c, f)
            ans = max(ans, max_four_dimenson_matrix)
    return ans

```

## Summary

- 转换成一个4*4的矩阵来计算 4种类型的最大值参考图例
- 注意矩阵的边界