---
title: "Counting Sundays"
date: 2022-11-21T14:45:03+08:00
draft: false
tags: ["Math","Python"]
categories: ["Project Euler"]
mathjax: true
---


[**Problem 19**](https://projecteuler.net/problem=19)

## Description

You are given the following information, but you may prefer to do some research for yourself.

- 1 Jan 1900 was a Monday.
- Thirty days has September,
  April, June and November.
  All the rest have thirty-one,
  Saving February alone,
  Which has twenty-eight, rain or shine.
  And on leap years, twenty-nine.
- A leap year occurs on any year evenly divisible by 4, but not on    century unless it is divisible by 400.

How many Sundays fell on the first of the month during the twentieth century (1 Jan 1901 to 31 Dec 2000)?

## Solution

```python
def solution():
    month_day_dict = {
        1: 31,
        2: 28,
        3: 31,
        4: 30,
        5: 31,
        6: 30,
        7: 31,
        8: 31,
        9: 30,
        10: 31,
        11: 30,
        12: 31,
    }

    def is_leap_year(year):
        if year % 4 == 0 and year % 400 == 0:
            return True
        return False

    init = 0
    count = 0
    for i in range(1900, 2001):
        if is_leap_year(i):
            month_day_dict[2] = 29
        for j in range(1, 13):
            for k in range(1, month_day_dict[j]):
                init += 1
                init %= 7
                if init == 0 and k == 1 and i >= 1901:
                    count += 1
                    print(f"The year {i} and month {j} the first day is sunday")
    return count
```

## Summary

简单总结下这道题

- 对于每个月的天数可以打表,常见的处理方式就是*字典*
- 判断年份是否闰年来确定 2月的天数
- 结果是获取每个月第一天是星期天的月份个数
- 初始值`1900年一月一号`是星期一和 求值区间是从`1901`年开始
