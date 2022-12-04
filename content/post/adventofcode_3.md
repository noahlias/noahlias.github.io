---
title: "Day 3: Rucksack Reorganization"
date: 2022-12-03T01:00:03+08:00
draft: false
tags: ["Advent", "Set"]
categories: ["Advent of Code"]
mathjax: true
---

[Day3](https://adventofcode.com/2022/day/3)

## 第一部分

大意就是给出下面的字符串,每个字符串等分为 2 个部分
找出第一部分和第二部分 共有的字符

字符串样例:

vJrwpWtwJgWrhcsFMMfFFhFp

jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL

PmmdzqPrVvPwwTWBwg

wMqvLMZHhHMvwLHjbvcjnnSBnvTQFn

ttgJtRGJQctTZtZT

CrZsJsPPZsGzwwsLwLmpwMDw

- 第一个字符串 第一部分是`vJrwpWtwJgWr`,第二部分是`hcsFMMfFFhFp`,共同出现字符是`p`.
- 第二个字符串 第一部分是`jqHRNqRjqzjGDLGL`,第二部分是`rsFMfFZSrLrFZsSL`,共同出现字符是`L`.
- 第三个字符串 第一部分是`PmmdzqPrV`,第二部分是`vPwwTWBwg`,共同出现字符是`P`.
- 第四个出现的字符是`v`
- 第五个出现的字符是`t`
- 第六个出现的字符是`s`

字符类型可以转换为优先级(`priority`)

- 小写字符`a`到`z` 优先级是 1 到 26
- 大写字符`A`到`Z` 优先级是 27 到 52

因此上述部分 16 (`p`), 38 (`L`), 42 (`P`), 22 (`v`), 20 (`t`), and 19 (`s`); 这些字符的总和是 `157`

## 第二部分

在第一部分的基础上做了部分修改 每三个字符串作为一个整体
找出这 3 个字符串的共有字符串

vJrwpWtwJgWrhcsFMMfFFhFp

jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL

PmmdzqPrVvPwwTWBwg

第一部分里面在这 3 个字符里面都出现的字符是`r`

wMqvLMZHhHMvwLHjbvcjnnSBnvTQFn

ttgJtRGJQctTZtZT

CrZsJsPPZsGzwwsLwLmpwMDw

第二部分共同出现的字符是`Z`
因此这部分的优先级总和为 18 (`r`) + 52 (`Z`)= `70`

## 解法

优先级转换

```python
def get_priority(letter:str):
    if letter.isupper():
        return ord(letter) - 38
    else:
        return ord(letter)- 96
```

### Part I

```python
def solution_part1(data):
    ans = []
    for i in data.split("\n"):
        first, second = i[: len(i) // 2], i[len(i) // 2 :]
        letter = set(first).intersection(set(second))
        ans.append(list(letter)[0])
    return sum((get_priority(i) for i in ans))
```

### Part II

```PYTHON
def solution_part2(data):
    ans = []
    res = [
        data.split("\n")[i * 3 : i * 3 + 3] for i in range(len(data.split("\n")) // 3)
    ]
    for j in res:
        first, second, third = j
        letter1 = set(first).intersection(set(second))
        letter = letter1.intersection(set(third))
        ans.append(list(letter)[0])
    return sum((get_priority(i) for i in ans))

```

## 总结

这个问题主要是需要处理字符串以及字符串的交集,
当然我利用了 python 中的`Set`的函数 `intersection`
来获取两个集合的交集来确定 共同出现的字符
