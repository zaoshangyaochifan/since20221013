---
title: "索引"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# 索引
1、对查询频繁的字段建立复合索引，利用索引覆盖优化冗余的复合索引；
1、利用索引优化数据库查询的时候，范围查询应该使用**闭包条件**，或者关键字**BETWEEN**、**like**，从而能够使用联合索引确定扫描边界，缩小扫描范围。