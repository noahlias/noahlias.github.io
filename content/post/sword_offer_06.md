---
title: "从尾到头打印链表"
date: 2022-02-23T12:55:03+08:00
draft: true
tags: ["栈","递归","链表","双指针"]
categories: ["Alogrithm"]
---

# 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例1**

```
**输入** head = [1,3,2]
**输出** [2,3,1]
```

**限制**

0 <= 链表长度 <= 10000

## 解法

### 递归

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        return self.reversePrint(head.next)+[head.val] if head else []
```

#### 解析

- 终止条件 head 为none 越过了链尾节点 否则返回空列表
- 递推操作：访问下一节点head.next
- 回溯阶段：

#### 复杂度分析

- **时间复杂度**O(N): 遍历链表，递归 N 次。
- **空间复杂度**O(N):系统递归需要使用 O(N)O(N) 的栈空间。


### 辅助栈

先入后出的可以用栈来实现

- **入栈**：遍历链表 入栈
- **出栈**：节点值出栈 存储数组返回

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        stack =[]
        while head:
            stack.append(head.val)
            head = head.next
        return stack[::-1]
```