---
title: "用两个栈实现队列"
date: 2022-02-23T12:55:03+08:00
draft: false
tags: ["队列","栈"]
categories: ["Alogrithm"]
---

# 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

## 设计

1. 加入队尾 appendTail() ： 将数字 val 加入栈 A 即可。
2. 删除队首deleteHead() ： 有以下三种情况
  > - 当栈 B 不为空： B中仍有已完成倒序的元素，因此直接返回 B 的栈顶元素。
  > - 否则，当 A 为空： 即两个栈都为空，无元素，因此返回 -1 。
  > - 否则： 将栈 A 元素全部转移至栈 B 中，实现元素倒序，并返回栈 B 的栈顶元素。

## 解法

```python
class CQueue:
    def __init__(self):
        self.A, self.B = [], []

    def appendTail(self, value: int) -> None:
        self.A.append(value)

    def deleteHead(self) -> int:
        if self.B:
            return self.B.pop()
        if not self.A:
            return -1
        while self.A:
            self.B.append(self.A.pop())
        return self.B.pop()
```