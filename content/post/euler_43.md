---
title: "Sub-string divisibility"
date: 2022-11-22T17:56:03+08:00
draft: false
tags: ["Math","Python","Pandigital"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 43**](https://projecteuler.net/problem=43)

## Description

The number, 1406357289, is a 0 to 9 pandigital number because it is made up of each of the digits 0 to 9 in some order, but it also has a rather interesting sub-string divisibility property.

Let $d_1$ be the 1st digit, $d_2$ be the 2nd digit, and so on. In this way, we note the following:

- $d_2d_3d_4=406$ is divisible by 2
- $d_3d_4d_5=063$ is divisible by 3
- $d_4d_5d_6=635$ is divisible by 5
- $d_5d_6d_7=357$ is divisible by 7
- $d_6d_7d_8=572$ is divisible by 11
- $d_7d_8d_9=728$ is divisible by 13
- $d_8d_9d_{10}=289$ is divisible by 17
Find the sum of all 0 to 9 pandigital numbers with this property.

## Solution

```python
def substring_divisibility():
    ''' 
    
    Get the rule definition for a substring
    pandigital numbers
    
    >>> substring_divisibility()
    >>> 16695334890
    '''

    import itertools

    pandigital_numbers = [
        "".join(map(str, i)) for i in itertools.permutations(list(range(10)))
    ]
    ans = 0
    for i in pandigital_numbers:
        sub_1 = int(i[1:4]) % 2
        sub_2 = int(i[2:5]) % 3
        sub_3 = int(i[3:6]) % 5
        sub_4 = int(i[4:7]) % 7
        sub_5 = int(i[5:8]) % 11
        sub_6 = int(i[6:9]) % 13
        sub_7 = int(i[7:10]) % 17
        if (
            sub_1 == 0
            and sub_2 == 0
            and sub_3 == 0
            and sub_4 == 0
            and sub_5 == 0
            and sub_6 == 0
            and sub_7 == 0
        ):  
            print(i)
            ans += int(i)
    return ans
```

## Summary

暴力解法,遍历所有全排列组合`泛位数`,根据规则筛选得到需要得数据