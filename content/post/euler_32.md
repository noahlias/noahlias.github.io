---
title: "Pandigital products"
date: 2022-11-22T13:31:03+08:00
draft: false
tags: ["Math","Python","Pandigital"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 32**](https://projecteuler.net/problem=32)

## Description

We shall say that an n-digit number is pandigital if it makes use of all the digits 1 to n exactly once; for example, the 5-digit number, 15234, is 1 through 5 pandigital.

The product 7254 is unusual, as the identity, 39 × 186 = 7254, containing multiplicand, multiplier, and product is 1 through 9 pandigital.

Find the sum of all products whose multiplicand/multiplier/product identity can be written as a 1 through 9 pandigital.

HINT: Some products can be obtained in more than one way so be sure to only include it once in your sum.

## Solution

The Formula just like: $a*b = c$

Two Situations:

- `a` has 1 digits,`b` has 4 digits and `c` has 4 digits
- `a` has 2 digits, `b` has 3 digits and `c` has 4 digits

**Brute force** calculation

```python

def pandigital_products():
    ''' 
    
    Calculat a*b = c 
    if str(a)+str(b)+str(c) == str(range(1,10))
    and the length is a nine digit number
    
    >>> pandigital_products()
    >>> 45228
    '''
    
    #* first situtation a 1digit number b is 4 digit number
    ans = {}
    for a in range(2,10):
        for b in range(100, 5000):
            if set(str(a)+str(b)+str(a*b)) == set(map(str,range(1,10))) \
                and len(str(a)+str(b)+str(a*b))==9:
               ans[f'{a}*{b}']=a*b 
    for a in range(10, 100):
        for  b in range(100,1000):
            if set(str(a)+str(b)+str(a*b)) == set(map(str,range(1,10)))\
                and len(str(a)+str(b)+str(a*b))==9:
               ans[f'{a}*{b}']=a*b 
    return sum(set(ans.values()))

```

## Summary

这个题目纯暴力求解 不过通过限制数值范围 
得到所有的符合要求的`泛位数`,记得提示中的需要过滤掉重复
数据即可