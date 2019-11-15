---
layout:     post
title:      快速排序
subtitle:   O(N * logN)时间复杂度
date:       2019-11-15
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - O(N * logN)
---


本文基于阿顾同学博客进行整理，原文链接https://blog.csdn.net/u010452388/article/details/81283998

阿顾同学把左神的课已经整理的很好的,这几个经典算法我在这里就当搬运工了。

基本思想是：从一个数组中随机选出一个数N，通过一趟排序将数组分割成三个部分，1、小于N的区域 2、等于N的区域 3、大于N的区域，然后再按照此方法对小于区的和大于区分别递归进行，从而达到整个数据变成有序数组。

## 图解流程

下面通过实例数组进行排序，存在以下数组:

