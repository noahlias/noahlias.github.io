---
title: "反转链表"
date: 2022-02-23T12:55:03+08:00
draft: false
tags: ["递归","链表"]
categories: ["Alogrithm","LeetCode"]
---

# 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点.

**示例**

```code
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

限制：

0 <= 节点个数 <= 5000


## 解法

```python

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur, pre = head, None
        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre
```

### 迭代(双指针)

- **复杂度分析O(N)**:遍历链表使用线性空间
- **空间复杂度O(1)**:变量 pre 和 cur 使用常数大小额外空间。

### 递归

1. 终止条件：当 cur 为空，则返回尾节点 pre （即反转链表的头节点）；
2. 递归后继节点，记录返回值（即反转链表的头节点）为 res ；
3. 修改当前节点 cur 引用指向前驱节点 pre ；
4. 返回反转链表的头节点 res

- **复杂度分析O(N)**:遍历链表使用线性空间
- **空间复杂度O(N)**:遍历链表的递归深度达到 NN ，系统使用 O(N)O(N) 大小额外空间。

```python


class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        def recur(cur, pre):
            if not cur: return pre     # 终止条件
            res = recur(cur.next, cur) # 递归后继节点
            cur.next = pre             # 修改节点引用指向
            return res                 # 返回反转链表的头节点
        return recur(head, None)       # 调用递归并返回

```
