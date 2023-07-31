---
title: "Day 3:Spiral Memory"
date: 2023-07-31T15:01:51+08:00
draft: false
tags: ["Advent"]
categories: ["Advent of Code","Haskell"]
mathjax: true
---

[Day3](https://adventofcode.com/2017/day/3)

螺旋记忆，这个标题就很有意思，实际它的实现也很有意思

## Part 1

第一部分大致讲解了就是一个螺旋的情况, 我用手绘图描述一下,基本就懂了
其实转换为一个流程理解起来也很简单

![spiral](https://i.imgur.com/tYQ4jD3.png)

不过参考了这个[链接]( https://stackoverflow.com/questions/57569623/coordinates-for-clockwise-outwards-spiral)

具体实现:

- 第一步定义上下左右
- 第二部确认是顺时针螺旋还是逆时针螺旋这个题目左和下是走2次
- 生成无限螺旋循环序列之后 从原点移动修改每一个点的坐标值
- 计算对应的曼哈顿距离即可得到结果等

```haskell
data Dir = R | D | L | U

spiralSeq :: Int -> [Dir]
spiralSeq n = rn R ++ rn U ++ rn1 L ++ rn1 D
  where
    rn = replicate n
    rn1 = replicate (n + 1)
spiral :: [Dir]
spiral = concatMap spiralSeq [1, 3 ..]

move :: (Int, Int) -> Dir -> (Int, Int)
move (x, y) = go
  where
    go R = (x + 1, y)
    go D = (x, y - 1)
    go L = (x - 1, y)
    go U = (x, y + 1)

spiralPos :: [(Int, Int)]
spiralPos = scanl move (0, 0) spiral
```

## Part 2

第二部分就是在第一部分上做了更多的工作,比如
它判断1个点的周围8个点是不是邻居 因此用于累加，其次用了**HashMAP**结构来记录每一个节点的累加值，其次读取这个无限递归螺旋哈希字典映射，从而判断找到比输入第一个大的值

```haskell
type Coordinate = (Int, Int)

type Spiral = Map.Map Coordinate Int

-- 创建一个用于获取坐标的所有邻居的函数
neighbors :: Coordinate -> [Coordinate]
neighbors (x, y) = [(x + dx, y + dy) | dx <- [-1, 0, 1], dy <- [-1, 0, 1], dx /= 0 || dy /= 0]

-- 创建一个用于从螺旋中获取特定坐标的值的函数
getValue :: Spiral -> Coordinate -> Int
getValue spiral coord
  | coord == (0, 0) = 1
  | otherwise = sum $ map (\c -> Map.findWithDefault 0 c spiral) $ neighbors coord

-- 递归生成螺旋值序列
spiralVals :: [Coordinate] -> Spiral -> [(Coordinate, Int)]
spiralVals (pos : positions) spiral = (pos, value) : spiralVals positions updatedSpiral
  where
    value = getValue spiral pos
    updatedSpiral = Map.insert pos value spiral

-- 修改part2以在新的Spiral结构中存储值
part2 xs = solve (read xs :: Int) spirals
  where
    spirals = spiralVals spiralPos (Map.singleton (0, 0) 1)
    solve target spirals = snd . head $ dropWhile ((<= target) . snd) spirals

```

## Summary

第一部分是参考**stackoverflow**来做的，第二部分是我提供的思路，但是具体实现借用了`GPT4` 来辅助我，具体不太懂Haskell里的一些hashmap如何实现和应用等以及有的地方需要利用惰性赋值来避免执行错误得不到正确的结果等
