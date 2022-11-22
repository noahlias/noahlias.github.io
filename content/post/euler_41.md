---
title: "Pandigital prime"
date: 2022-11-22T11:15:03+08:00
draft: false
tags: ["Math","Python","Pandigital","Prime"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 41**](https://projecteuler.net/problem=41)

## Description

We shall say that an n-digit number is pandigital if it makes use of all the digits 1 to n exactly once. For example, 2143 is a 4-digit pandigital and is also prime.

What is the largest n-digit pandigital prime that exists?

## Solution

```python
def is_prime(n):
    ''' 
    
    Use the prime algorithm to judge whether a number is prime
    Time complexity is O(sqrt(n)).
    
    '''
    q = 2
    while q * q <= n:
        if n % q == 0:
            return False
        q += 1
    return True


def pandigital_prime():
    ''' 
    
    Use a trick to get the all pandigital numbers
    with the number permutations the all pandigital numbers
    count is 46233.so we can limit the range to avoid the 
    execute too many times
    
    >>> pandigital_prime()
    >>> 7652413
    '''
    ans = 0
    import itertools
    for j in range(2, 10):
        pandigital = [
            "".join(map(str, i)) for i in itertools.permutations(list(range(1, j)))
        ]
        for n in pandigital:
            if is_prime(int(n)):
                ans = max(ans, int(n))
    return ans
```

## Summary

我采取的方法是暴力解法,先获取所有的**pandigital**数字(通过全排列的方式)
之后用素数判断来确定最大的`泛位数`
