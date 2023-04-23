---
title: "Trick in Python"
date: 2023-04-23T10:10:03+08:00
draft: false
tags: ["python"]
categories: ["Life"]
mathjax: true
---

下面会介绍一下有趣的技巧,来源于**PythonDiscord**[^2]和**Reddit**[^1]

- [x] [**F-strings**](#f-strings)
- [x] [**List Comprehensions**](#list-comprehensions)
- [x] [**DataClass**](#dataclass)
- [x] [**Matrix**](#matrix)

## F-strings

看到一个有趣的用法，之前自己没见过[^1]

```python
foos = [1, 2]
bar, qaz = 3, 3
f"{(len(foos), bar + qaz)=}"
```

等同于

```python
(len(foos), bar + qaz)=(2, 6)
```

### numeric

```python
digits = 2
f“{1/3:.{digits}f}“
```

更多有趣的`f-string`格式可去这里看[**PyFormat**](https://pyformat.info/)

## List Comprehensions

这个就很常见了,列表表达式

- *list*
- *dict*

最常见的就是这种oneline code了

```python
person_dict = {person.name:person for person in persons}
```

### FIZBUZZ

这个也是在**Reddit**上评论里面看到的一个小trick

```python
mappings = {
    (True, True): "FizzBuzz",
    (True, False): "Fizz",
    (False, True): "Buzz",
}
for i in range(1, 21):
    print(mappings.get((i % 3 == 0, i % 5 == 0), i))
```

## DataClass

这个feature很有意思[^3],有很多有意思的功能在创建类的时候
还可以通过`slots`来减少内存使用,提高效率等[^4]

## Matrix

比如有一个场景,你需要对矩阵进行顺时针旋转90

```python
[
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
]
```

转变为

```python
[
  [7, 4, 1],
  [8, 5, 2],
  [9, 6, 3],
]
```

用一行代码来实现

```shell
>>> matrix = [[1, 2, 3],
...           [4, 5, 6],
...           [7, 8, 9]]
>>> list(map(list, zip(*matrix[::-1])))
... [[7, 4, 1],
...  [8, 5, 2],
...  [9, 6, 3]]
```

其实这种也可以通过**numpy**来处理 但是会增加额外的空间消耗等

```python
np.rot90(matrix, k=1, axes=(1, 0))
>>>array([[7, 4, 1],
       [8, 5, 2],
       [9, 6, 3]])
```

[^1]: [reddit](https://www.reddit.com/r/Python/comments/12tr2sn/pythoneers_here_what_are_some_of_the_best_python/)
[^2]: [PythonDiscord](https://www.pythondiscord.com/)
[^3]: 该模块提供了一个装饰器和函数，用于自动向用户定义的类添加生成的特殊方法，例如__init__()和__repr__(),详情参考[dataclass](https://docs.python.org/3/library/dataclasses.html)。
[^4]: [use_slots_true_to_imporve_performance](https://faun.pub/improve-python-class-performance-using-slots-d3e986a282cf)
