---
title: "表示数值的字符串"
date: 2022-02-23T12:55:03+08:00
draft: false
tags: ["字符串","有限状态自动机"]
categories: ["Alogrithm"]
author: "noahlias"
---

# 数值字符串

## 解法

本题使用有限状态自动机。根据字符类型和合法数值的特点，先定义状态，再画出状态转移图，最后编写代码即可。

字符类型：

空格 「 」、数字「 0—90—9 」 、正负号 「 ++, -− 」 、小数点 「 .. 」 、幂符号 「 ee, EE 」 。

状态定义：

按照字符串从左到右的顺序，定义以下 9 种状态。

开始的空格
幂符号前的正负号
小数点前的数字
小数点、小数点后的数字
当小数点前为空格时，小数点、小数点后的数字
幂符号
幂符号后的正负号
幂符号后的数字
结尾的空格
结束状态：

合法的结束状态有 2, 3, 7, 8 。

初始化：

状态转移表 states ： 设 states[i] ，其中 i 为所处状态， states[i] 使用哈希表存储可转移至的状态。键值对 (key, value) 含义：输入字符 key ，则从状态 i 转移至状态 value 。
当前状态 p ： 起始状态初始化为 p = 0 。
状态转移循环： 遍历字符串 s 的每个字符 c 。

记录字符类型 t ： 分为四种情况。
当 c 为正负号时，执行 t = 's' ;
当 c 为数字时，执行 t = 'd' ;
当 c 为 e 或 E 时，执行 t = 'e' ;
当 c 为 . 或 空格 时，执行 t = c （即用字符本身表示字符类型）;
否则，执行 t = '?' ，代表为不属于判断范围的非法字符，后续直接返回 falsefalse 。
终止条件： 若字符类型 t 不在哈希表 states[p] 中，说明无法转移至下一状态，因此直接返回 falsefalse 。
状态转移： 状态 p 转移至 states[p][t] 。

```python
class Solution:
    def isNumber(self, s: str) -> bool:
        states = [
            { ' ': 0, 's': 1, 'd': 2, '.': 4 }, # 0. start with 'blank'
            { 'd': 2, '.': 4 } ,                # 1. 'sign' before 'e'
            { 'd': 2, '.': 3, 'e': 5, ' ': 8 }, # 2. 'digit' before 'dot'
            { 'd': 3, 'e': 5, ' ': 8 },         # 3. 'digit' after 'dot'
            { 'd': 3 },                         # 4. 'digit' after 'dot' (‘blank’ before 'dot')
            { 's': 6, 'd': 7 },                 # 5. 'e'
            { 'd': 7 },                         # 6. 'sign' after 'e'
            { 'd': 7, ' ': 8 },                 # 7. 'digit' after 'e'
            { ' ': 8 }                          # 8. end with 'blank'
        ]
        p = 0                           # start with state 0
        for c in s:
            if '0' <= c <= '9': t = 'd' # digit
            elif c in "+-": t = 's'     # sign
            elif c in "eE": t = 'e'     # e or E
            elif c in ". ": t = c       # dot, blank
            else: t = '?'               # unknown
            if t not in states[p]: return False
            p = states[p][t]
        return p in (2, 3, 7, 8)
```

## 偷懒解法

```python
class Solution:
    def isNumber(self, s: str) -> bool:
        res = True
        try:
            x = float(s.strip())
        except:
            res = False
        return res
```