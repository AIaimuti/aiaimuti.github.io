---
layout:     post
title:      归并排序
subtitle:   O(N * logN)时间复杂度
date:       2019-11-19
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - O(N * logN)
---


本文基于阿顾同学博客进行整理，原文链接https://blog.csdn.net/u010452388/article/details/81218540

## 图解流程

归并排序即采用二分法不断分解，分解到单个数的时候开始合并，最终合并为一个有序数组。

分解原理：每次找到数组中点（L + （R - L）>>2）（这样有两个优点1、防溢出 2、速度快），将当前数组划分为两个数组，递归划分最终分解成一个数。

合并原理：归并的每一次合并都是将两个有序组合并为一个有序组，合并好后的有序组，再和另外的有序组继续合并，最终可以得到一个完整的有序数组

