---
title: LeetCode/LintCode刷题之排序
date: 2017-3-17 16:50:03
comments: true
mathjax: true
categories:
	- 刷题
	- 面试
tags: 
	- C++
	- Python
---
# 排序
## Merge Sorted Array

## Question

- leetcode: [Merge Sorted Array | LeetCode OJ](https://leetcode.com/problems/merge-sorted-array/)
- lintcode: [(6) Merge Sorted Array](http://www.lintcode.com/en/problem/merge-sorted-array/)

```
Given two sorted integer arrays A and B, merge B into A as one sorted array.

Example
A = [1, 2, 3, empty, empty], B = [4, 5]

After merge, A will be filled as [1, 2, 3, 4, 5]

Note
You may assume that A has enough space (size that is greater or equal to m + n)
to hold additional elements from B.
The number of elements initialized in A and B are m and n respectively.
```

### 题解

因为本题有 in-place 的限制，故必须从数组末尾的两个元素开始比较；否则就会产生挪动，一旦挪动就会是 $O(n^2)$ 的。
自尾部向首部逐个比较两个数组内的元素，取较大的置于数组 A 中。由于 A 的容量较 B 大，故最后 `m == 0` 或者 `n == 0` 时仅需处理 B 中的元素，因为 A 中的元素已经在 A 中，无需处理。

### C++

```c++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        int index = m + n - 1;
        while (m > 0 && n > 0) {
            if (A[m - 1] > B[n - 1]) {
                A[index] = A[m - 1];
                --m;
            } else {
            A[index] = B[n - 1];
            --n;
            }
        --index;
        }
        // 如果B非空，因为A中的元素已经在A中，m==0时，A中元素位置已调整好，B中元素不定
        while (n > 0) {
            A[index] = B[n - 1];
            --n;
            --index;
        }
    }
};
```
### 复杂度分析

最坏情况下需要遍历两个数组中所有元素，时间复杂度为 $O(m+n)$. 空间复杂度 $O(1)$.
## Merge Two Sorted Lists

## Question

- leetcode: [Merge Two Sorted Lists | LeetCode OJ](https://leetcode.com/problems/merge-two-sorted-lists/)
- lintcode: [(165) Merge Two Sorted Lists](http://www.lintcode.com/en/problem/merge-two-sorted-lists/)

### Problem Statement

Merge two sorted (ascending) linked lists and return it as a new sorted list.
The new sorted list should be made by splicing together the nodes of the two
lists and sorted in ascending order.

#### Example

Given `1->3->8->11->15->null`, `2->null` , return `1->2->3->8->11->15->null`.

### 题解

此题为两个链表的合并，合并后的表头节点不一定，故应联想到使用`dummy`节点。链表节点的插入主要涉及节点`next`指针值的改变，两个链表的合并操作则涉及到两个节点的`next`值变化，若每次合并一个节点都要改变两个节点`next`的值且要对`NULL`指针做异常处理，势必会异常麻烦。嗯，第一次做这个题时我就是这么想的... 下面看看相对较好的思路。

首先`dummy`节点还是必须要用到，除了`dummy`节点外还引入一个`curr`节点充当下一次合并时的头节点。在`l1`或者`l2`的某一个节点为空指针`NULL`时，退出`while`循环，并将非空链表的头部链接到`curr->next`中。

### C++
