---
layout:     post
title:      C++容器
subtitle:   基础内容
date:       2019-11-11
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - C++
---

## map和set有什么区别，分别又是怎么实现的

map和set都是C++的关联容器，其底层实现都是红黑树（RB-Tree）。由于 map 和set所开放的各种操作接口，RB-tree 也都提供了，所以几乎所有的 map 和set的操作行为，都只是转调 RB-tree 的操作行为。
map和set区别在于：
  1. map中的元素是key-value（关键字—值）对：关键字起到索引的作用，值则表示与索引相关联的数据；Set与之相对就是关键字的简单集合，set中每个元素只包含一个关键字。
  2. **set的迭代器是const的，不允许修改元素的值；map允许修改value，但不允许修改key。** 其原因是因为map和set是根据关键字排序来保证其有序性的，如果允许修改key的话，那么首先需要删除该键，然后调节平衡，再插入修改后的键值，调节平衡，如此一来，严重破坏了map和set的结构，导致iterator失效，不知道应该指向改变前的位置，还是指向改变后的位置。所以STL中将set的迭代器设置成const，不允许修改迭代器的值；而map的迭代器则不允许修改key值，允许修改value值。
  3. **map支持下标操作，set不支持下标操作。** map可以用key做下标，map的下标运算符[ ]将关键码作为下标去执行查找，如果关键码不存在，则插入一个具有该关键码和mapped_type类型默认值的元素至map中，因此下标运算符[ ]在map应用中需要慎用，const_map不能用，只希望确定某一个关键值是否存在而不希望插入元素时也不应该使用，mapped_type类型没有默认值也不应该使用。如果find能解决需要，尽可能用find。
  
## STL的allocaotr

STL的分配器用于封装STL容器在内存管理上的底层细节。在C++中，其内存配置和释放如下：
  + new运算分两个阶段：(1)调用::operator new配置内存;(2)调用对象构造函数构造对象内容
  + delete运算分两个阶段：(1)调用对象析构函数；(2)掉员工::operator delete释放内存
为了精密分工，STL allocator将两个阶段操作区分开来：
  + 内存配置有alloc::allocate()负责，内存释放由alloc::deallocate()负责；
  + 对象构造由::construct()负责，对象析构由::destroy()负责。
同时为了提升内存管理的效率，减少申请小内存造成的内存碎片问题，SGI STL采用了两级配置器：
  + 第一级空间配置器直接使用malloc()、realloc()、free()函数进行内存空间的分配和释放，当分配的空间大小超过128B时，会使用第一级空间配置器。
  + 第二级空间配置器采用了内存池技术，通过空闲链表来管理内存，当分配的空间大小小于128B时，将使用第二级空间配置器。
