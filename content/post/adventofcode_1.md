---
title: "Day 1:Calorie Counting"
date: 2022-12-02T19:41:03+08:00
draft: false
tags: ["Advent"]
categories: ["Advent of Code"]
mathjax: true
---

[Day1](https://adventofcode.com/2022/day/1)

## 描述

大意就是给出如下的数据代表不同的食物消耗的`卡路里`(**Calories**)
样例:

|       |
| ----- |
| 1000  |
| 2000  |
| 3000  |
|       |
| 4000  |
|       |
| 5000  |
| 6000  |
|       |
| 7000  |
| 8000  |
| 9000  |
|       |
| 10000 |

- 第一部分 一共是$1000+2000+3000=6000$ 卡路里消耗
- 第二部分 一共是$4000$
- 第三部分 一共是$5000+6000=11000$
- 第四部分 一共是$7000+8000+9000=24000$
- 第五部分 一共是$10000$

选择最大的卡路里消耗(`第四部分`)
$$\max(6000,4000,11000,24000,10000)=24000$$

## 解法

通用方法获取数据集

```python
import requests

def get_day_data(n):
    url = f"https://adventofcode.com/2022/day/{str(n)}/input"
    cookies = {
        "session": "your session cookie"
    }
    data = requests.get(url, cookies=cookies)
    return data.text[: len(data.text) - 1]

```

解法思路：

1. 第一部分先解析2个换行符的数据 之后做完一个整体计算,遍历一下即可得到结果
2. 第二部分 算前三最大的 可以用个trick 使用`heap`的`nlargest`来完成这部分

### 第一部分

```python
d = [i for i in get_day_data(1).split("\n\n")]
ans = []
for j in d:
    if len(j.split("\n")) == 1:
        ans.append(int(j))
    else:
        temp = sum(map(int, j.split("\n")))
        ans.append(temp)
max(ans)
```

### 第二部分

```python
import heapq
c = heapq.heapify(ans)
sum(heapq.nlargest(3,ans))
```

## 总结

这个题有很多解法,在`PythonDiscord` 上也有很多相关的讨论.
有很多很酷的想法比如一行完成这个问题
