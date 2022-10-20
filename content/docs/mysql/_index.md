---
title: "MySQL"
weight: 2
bookFlatSection: true
bookToc: true
bookHidden: false
bookCollapseSection: false
bookComments: true
bookSearchExclude: false
---

# MySQL

## JOIN有几种算法？
在**有索引的情况下走索引**，没有索引的情况下使用NLJ、BNL两种算法：
1. Nested Loop Join 每行依次比较
2. Block Nested Loop Join 使用sort buffer，减少IO次数，但不会减少比较次数

## 为什么要将JOIN拆分成多次单表操作？
1. 单表对缓存更友好，可以利用缓存增加查询速度；