---
title: "Day 2: Corruption Checksum"
date: 2023-07-31T14:51:48+08:00
draft: false
tags: ["Advent"]
categories: ["Advent of Code","Haskell"]
mathjax: true
---

[Day2](https://adventofcode.com/2017/day/2)

校验和这个标题很符合这个第一部分题目

## Part 1

第一个问题就是找出二维数组里面的每一个元素最大和最小元素的差之后做个统计下

具体实现:

```haskell
preprocessing :: [Char] -> [[Int]]
preprocessing xs = [[read x :: Int | x <- xs] | xs <- map (splitOn "\t") (splitOn "\n" xs)]
-- list comprehension
part1 xs = sum [maximum x - minimum x | x <- preprocessing xs]

```

## Part 2

第二部分就是在第一部分上将元素处理改成了唯2个可以整除的元素

```haskell
solution xs = head [x `div` y | x <- sortedXs, y <- sortedXs, x /= y, x `mod` y == 0]
  where
    sortedXs = reverse (sort xs)

part2 xs = sum [solution x | x <- preprocessing xs]
```

## Summary

第二天的题目感觉太简单了,也就是考察一些字符串处理其实也是数据处理等

## Reference

有一些有趣的function 比如`maximum` 和`minimum` 以及 `splitOn`
这些函数是真的很方便但是它的实现也很有意思
比如

```haskell
maximum :: (Ord a) => [a] -> a
maximum [] = error "maximum of empty list"
maximum [x] = x
maximum (x:xs) = max x (maximum xs)
```
