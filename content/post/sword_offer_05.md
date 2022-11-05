---
title: "替换空格"
date: 2022-02-23T12:55:03+08:00
draft: false
tags: ["剑指offer","Alorightms"]
categories: ["Alogrithm"]
author: "noahlias"
---

# 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

**示例 1**

```
**输入**：s = "We are happy."
输出："We%20are%20happy."
```

限制：

0 <= s 的长度 <= 10000

## 解法

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        return s.replace(' ','%20')
```

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        res = []
        for c in s:
            if c == ' ': res.append("%20")
            else: res.append(c)
        return "".join(res)
```

## 思路

### 遍历添加

#### 算法流程

- 初始化一个 list (Python) / StringBuilder (Java) ，记为 res;
- 遍历列表 s 中的每个字符 c:
1.当 c 为空格时：向 res 后添加字符串 "%20";
2.当 c 不为空格时：向 res 后添加字符 c;
- 将列表 res 转化为字符串并返回。

#### 复杂度分析

- **时间复杂度 O(N)**： 遍历使用 O(N) ，每轮添加（修改）字符操作使用 O(1);
- **空间复杂度 O(N)**： Python 新建的 list 和 Java 新建的 StringBuilder 都使用了线性大小的额外空间。


### 原地修改
在 C++ 语言中， string 被设计成「可变」的类型([参考资料](https://stackoverflow.com/questions/28442719/are-c-strings-mutable-unlike-java-strings))

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0, len = s.size();
        // 统计空格数量
        for (char c : s) {
            if (c == ' ') count++;
        }
        // 修改 s 长度
        s.resize(len + 2 * count);
        // 倒序遍历修改
        for(int i = len - 1, j = s.size() - 1; i < j; i--, j--) {
            if (s[i] != ' ')
                s[j] = s[i];
            else {
                s[j - 2] = '%';
                s[j - 1] = '2';
                s[j] = '0';
                j -= 2;
            }
        }
        return s;
    }
};

```