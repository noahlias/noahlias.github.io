---
title: "Day 4: Camp Cleanup"
date: 2022-12-05T01:46:03+08:00
draft: false
tags: ["Advent", "Set"]
categories: ["Advent of Code"]
mathjax: true
---

[Day4](https://adventofcode.com/2022/day/4)

## 简介

## 第一部分

大意就是找出下面配对序列中 范围有完全包含另一个序列的个数

- 2-4,6-8
- 2-3,4-5
- 5-7,7-9
- 2-8,3-7
- 6-6,4-6
- 2-6,4-8

上面给出的配对中 只有`2-8, 3-7` 和`6-6,4-6` 是完全包含的 因此存在 2 种

## 第二部分

寻找有重叠部分(`overlap at all`)的配对

- 5-7,7-9 :重叠部分为 7.
- 2-8,3-7 :重叠部分为 3 到 7.
- 6-6,4-6 :重叠部分为 6.
- 2-6,4-8 :重叠部分为 是 4, 5, and 6

因此包含此种配对一共有四种

## 解法

### Part I

```python
def solution_part_1(data):
    count = 0
    for i in data.split("\n"):
        first, second = i.split(",")
        first_list = list(
            map(str, range(int(first.split("-")[0]), int(first.split("-")[1]) + 1))
        )
        second_list = list(
            map(str, range(int(second.split("-")[0]), int(second.split("-")[1]) + 1))
        )
        interset_set = set(first_list).intersection(set(second_list))
        if interset_set == set(first_list) or interset_set == set(second_list):
            count += 1
    return count

```

### Part II

```python

def solution_part_2(data):
    count = 0
    for i in data.split("\n"):
        first, second = i.split(",")
        first_list = list(
            map(str, range(int(first.split("-")[0]), int(first.split("-")[1]) + 1))
        )
        second_list = list(
            map(str, range(int(second.split("-")[0]), int(second.split("-")[1]) + 1))
        )
        interset_set = set(first_list).intersection(set(second_list))
        if len(interset_set) > 0:
            count += 1
    return count
```

## 总结

- 问题可以转换数学集合中的交集和子集判断
