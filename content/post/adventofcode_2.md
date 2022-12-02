---
title: "Day 2:Rock Paper Scissors"
date: 2022-12-03T01:00:03+08:00
draft: false
tags: ["Advent", "Game"]
categories: ["Advent of Code"]
mathjax: true
---

[Day2](https://adventofcode.com/2022/day/2)

## 描述

大意就是石头剪刀布这个游戏分为对手和你
得分分为 你出手的形状得分 和最终胜利时的得分总和

出手形状得分:

- `石头`:1分
- `布`: 2分
- `剪刀`:3分

胜利得分:

- **胜局**: 6分
- **平局**: 3分
- **输局**: 0分

## 解法

解法思路：

1. 第一部分规则是判断是否要赢或者输或者平局，得出获胜积分,此时选择积分是已经确定的
2. 第二部分是规则 需要赢或者输或者平局 出对应的石头、剪刀、布 从而影响选择得分，而获胜积分此时
是确定的

下面我做的是暴力解法,可以优化

### 第一部分

```python
from typing import List
def solution_two(two:List):
    game_ans = []
    for i in two :
        opponent , myself = i
        select_dict = {'X': 1,'Y':2,'Z':3}
        win_dict = {'W':6,'L':0,'D':3}
        opp_dict = {'A':'Rock','B':'Paper','C':'Scissors'}
        own_dict = {'X':'Rock','Y':'Paper','Z':'Scissors'}
        if own_dict[myself] == 'Rock':
            if opp_dict[opponent]=='Paper':
                win_score = win_dict['L']
            elif opp_dict[opponent]=='Scissors':
                win_score = win_dict['W']
            else:
                win_score = win_dict['D']
        if own_dict[myself] == 'Paper':
            if opp_dict[opponent]=='Scissors':
                win_score = win_dict['L']
            elif opp_dict[opponent]=='Rock':
                win_score = win_dict['W']
            else:
                win_score = win_dict['D']
        if own_dict[myself] == 'Scissors':
            if opp_dict[opponent]=='Rock':
                win_score = win_dict['L']
            elif opp_dict[opponent]=='Paper':
                win_score = win_dict['W']
            else:
                win_score = win_dict['D']
        round_score = select_dict[myself] + win_score
        game_ans.append(round_score)
    return game_ans
```

### 第二部分

```python
def solution_two_another(two:List):
    game_ans = []
    for i in two :
        opponent , myself = i
        select_dict = {'X': 1,'Y':2,'Z':3}
        win_dict = {'W':6,'L':0,'D':3}
        opp_dict = {'A':'Rock','B':'Paper','C':'Scissors'}
        own_dict = {'X':'L','Y':'D','Z':'W'}
        if own_dict[myself] == 'L':
            if opponent == 'A':
                new_type = 'Z'
            if opponent == 'B':
                new_type = 'X'
            if opponent == 'C':
                new_type = 'Y'
            select_score = select_dict[new_type]
            win_score = win_dict['L']
        if own_dict[myself] == 'D':
            if opponent == 'A':
                new_type = 'X'
            if opponent == 'B':
                new_type = 'Y'
            if opponent == 'C':
                new_type = 'Z'
            select_score = select_dict[new_type]
            win_score = win_dict['D']
        if own_dict[myself] == 'W':
            if opponent == 'A':
                new_type = 'Y'
            if opponent == 'B':
                new_type = 'Z'
            if opponent == 'C':
                new_type = 'X'
            select_score = select_dict[new_type]
            win_score = win_dict['W']
        round_score = select_score + win_score
        game_ans.append(round_score)
    return game_ans
```

## 总结

这个题应该提前计算好每一种情况的值来预处理 简化结果 另外
我看**Discord** 上有人给出一行的解决方案

```python
print(
    sum(
        (r := (M := "XYZABC".index)(L[2]))
        + [1 + 3 * (r == (o := M(L[0]) - 3)), 7][r == (o + 1) % 3]
        + 1j * [p := [3, 1, 2][o], 4 + o, 11 - p - o][r]
        for L in open("input/02.txt")
    )
)

```

**PreComputed**:

```python
with open("the.txt",'r') as f:
    k = f.read().split('\n')
    l = []
    for i in k:
        l.append(i.split(' '))
    m = 0
    for i in l:
        if ''.join(i) == "AX":
            m += 4
        if ''.join(i) == "AY":
            m += 8
        if ''.join(i) == "AZ":
            m  += 3
        if ''.join(i) == "BX":
            m += 1
        if ''.join(i) == "BY":
            m += 5
        if ''.join(i) == "BZ":
            m += 9
        if ''.join(i) == "CX":
            m += 7
        if ''.join(i) == "CY":
            m += 2
        if ''.join(i) == "CZ":
            m += 6
print(m)
```
