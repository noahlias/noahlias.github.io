---
title: "Day 6: Tuning Trouble"
date: 2022-12-06T15:16:03+08:00
draft: false
tags: ["Advent"]
categories: ["Advent of Code"]
mathjax: true
---

[Day6](https://adventofcode.com/2022/day/6)

## 描述

例如，假设你收到以下数据流缓冲区。

`mjqjpqmgbljsphdztnvjfqwrcgsmlb`
在收到前三个字符（mjq）后，还没有收到足够的字符来寻找标记。第一次可能出现的标记是在收到第四个字符之后，使最近的四个字符成为mjqj。因为j是重复的，这不是一个标记。

标记的第一次出现是在第七个字符到达之后。一旦它出现，收到的最后四个字符就是jpqm，这都是不同的。在这种情况下，你的子程序应该报告值为7，因为在处理了7个字符后，第一个包开始标记就完成.

- `bvwbjplbgvbhsrlpgdmjqwftvncz`: 报告第5个字符
- `nppdvjthqldpwncqszvftbrmjlhg`: 报告第6个字符
- `nznrnfrfntjfmvfwmzdfjlvtqnbhcprsg`: 报告第10个字符
- `zcfzfwzzqfrljwzlrfnpqdbhtmscgvjw`: 报告第11个字符

## 解法

### 第一部分

```python
def solution_day6_part1(s:str):
    i = 0
    while len(set(s[i:i+4]))!=4:
        i+=1
    return i+4
```

### 第二部分

```python
def solution_day6_part2(s:str):
    i = 0
    while len(set(s[i:i+14]))!=14:
        i+=1
    return i+14
```

## 总结

这个题目很简单就是找到第一次出现不为重复的字符串标记