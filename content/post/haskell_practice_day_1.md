---
title: "Day 1:Inverse Captcha"
date: 2023-07-31T11:53:27+08:00
draft: false
tags: ["Advent"]
categories: ["Advent of Code","Haskell"]
mathjax: true
---

[Day1](https://adventofcode.com/2017/day/1)

好久没做题了,这次是一次函数式编程语言的回归与练习

## Part 1

第一部分大致讲解了就是一个旋转的情况, 我用手绘图描述一下,基本就懂了

![haskell_rotate](https://i.imgur.com/xwXeSAk.png)

具体实现:

```haskell
rotate :: Int -> [a] -> [a]
rotate _ [] = []
rotate n xs = zipWith const (drop n (cycle xs)) xs
```

## Part 2

第二部分就是在第一部分上*rotate*的时候修改了窗口长度,改成了字符串的长度的一半,基本没啥区别.

## Summary

第一天的题目就是考察数组处理,如何在**iterator**产生新的数组,Haskell的实现很巧妙,它用一个循环数组,根据窗口删除位置获取前n个元素上面那个图的链,再用拉链函数拼接原始数组,完美获取了这个旋转函数。

这里的函数式让我想起了**SICP** 中的eval and apply,借由下面这一张图来理解它吧

![lambda](/eval-apply.gif)

## Reference

const是一个很有用的函数,用于忽略第二个参数 返回对一个参数

```haskell
const :: a -> b -> a
```
