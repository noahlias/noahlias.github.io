---
title: "Pandigital multiples"
date: 2022-11-21T19:48:03+08:00
draft: false
tags: ["Math","Python"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 38**](https://projecteuler.net/problem=38)

## Description

Take the number 192 and multiply it by each of 1, 2, and 3:

- 192 × 1 = 192
- 192 × 2 = 384
- 192 × 3 = 576

By concatenating each product we get the 1 to 9 pandigital, 192384576. We will call 192384576 the concatenated product of 192 and (1,2,3)

The same can be achieved by starting with 9 and multiplying by 1, 2, 3, 4, and 5, giving the pandigital, 918273645, which is the concatenated product of 9 and (1,2,3,4,5).

What is the largest 1 to 9 pandigital 9-digit number that can be formed as the concatenated product of an integer with (1,2, ... , n) where n > 1?

## Solution

```python
def helper(n):
    '''

    Brute force to get the answer
    actually it can limit the range up to a certain number
    In the n=2 we can limit the number to 9233 to 9487

    '''
    ans = []
    default_digit = 9
    for i in range(10 ** (default_digit // n - 1), 10 ** (default_digit // n)):
        data_str = "".join([str(j * i) for j in range(1, n + 1)])
        if len(data_str) == 9 and set(data_str) == set(list(map(str, range(1, 10)))):
            ans.append(data_str)
    return ans


def solution():
    data = [helper(i) for i in range(2, 6)]
    new_data = [item for sublist in data for item in sublist]
    return sorted(new_data)
```

**Answer**
![eluer_38](/plot/eluer_38.jpg)

## Summary
第一次做这个的时候实际已经写出答案了 我在存储数字的时候写失误了 导致我用暴力写的时候
没得到正确答案 导致我浪费很多时间在`debug`上面
- 边界在`[2,6]`之间
- 4位数判断时候边界在`[9233,9487]`之间

## References

[**欧拉项目题解**](https://pe.metaquant.org/pe038.html)