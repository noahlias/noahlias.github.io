---
title: "Permuted multiples"
date: 2022-12-02T19:41:03+08:00
draft: false
tags: ["Math","Python"]
categories: ["Project Euler"]
mathjax: true
---

[**Problem 52**](https://projecteuler.net/problem=5)

## Description

It can be seen that the number, 125874, and its double, 251748, contain exactly the same digits, but in a different order.

Find the smallest positive integer, x, such that 2x, 3x, 4x, 5x, and 6x, contain the same digits.

## Solution

```python
def solution():

    for i in range(1, 1000000):
        a = str(i * 2)
        b = str(i * 3)
        c = str(i * 4)
        d = str(i * 5)
        e = str(i * 6)
        if (
            sorted(a) == sorted(b)
            and sorted(a) == sorted(c)
            and sorted(a) == sorted(d)
            and sorted(a) == sorted(e)
        ):
            return i
```

## Summary

It limit the number range to one billion.

five numbers:

- $142857$
- $285714$
- $428751$
- $571428$
- $857142$
