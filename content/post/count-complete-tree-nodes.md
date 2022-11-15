---
title: "Count Complete Tree Nodes"
date: 2022-11-15T23:55:03+08:00
draft: false
tags: ["每日一题","LeetCode_Medium","BinaryTree"]
categories: ["DailyChallenge"]
---

[222.Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)
## Description

Given the `root` of a complete binary tree, return the number of the nodes in the tree.

According to [**Wikipedia**](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees), every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and $2^h$ nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

Constraints:
- The number of nodes in the tree is in the range $[0, 5 * 10^4]$.
- $0$<= Node.val <= $5 * 10^4$
- The tree is guaranteed to be complete.

## Solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        return self.countNodes(root.left)+1+self.countNodes(root.right)
```

## Summary

I think this problem  more like `easy`. and the complete binary tree node count is $2^h-1$.

- The common solution is :**Recursive**
- The inital boundary is none nodes need to be zero.