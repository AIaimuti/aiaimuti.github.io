---
layout:     post
title:      链表
subtitle:   相关知识及题目
date:       2019-11-20
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 链表
---
## 面试时链表解题的方法论
1. 对于笔试，不用太在乎空间复杂度，一切为了时间复杂度
2. 对于面试，时间复杂度依然放在第一位，但是一定要找到空间最省的方法

## 链表相关题目主要就是两个思路

1. 使用辅助空间，数组，哈希表等

2. 使用快慢指针。

## 哈希表简单介绍

1. 哈希表在使用层面上可以理解为一种集合结构
2. 如果只有key，没有伴随数据value，可以使用HashSet结构(C++中叫UnOrderedSet)
3. 如果既有key，又有伴随数据value，可以使用HashMap结构(C++中叫UnOrderedMap)
4. 有无伴随数据，是HashMap和HashSet唯一的区别，底层的实际结构是一回事
5. 使用哈希表增(put)、删(remove)、改(put)和查(get)的操作，可以认为时间复杂度为O(1)，但是常数时间比较大
6. 放入哈希表的东西，如果是基础类型，内部按值传递，内存占用就是这个东西的大小
7. 放入哈希表的东西，如果不是基础类型，内部按引用传递，内存占用是这个东西内存地址的大小.

## 有序表简单介绍

1. 有序表在使用层面上可以理解为一种集合结构
2. 如果只有key，没有伴随数据value，可以使用TreeSet结构(C++中叫OrderedSet)
3. 如果既有key，又有伴随数据value，可以使用TreeMap结构(C++中叫OrderedMap)
4. 有无伴随数据，是TreeSet和TreeMap唯一的区别，底层的实际结构是一回事
5. 有序表和哈希表的区别是，有序表把key按照顺序组织起来，而哈希表完全不组织
6. 红黑树、AVL树、size-balance-tree和跳表等都属于有序表结构，只是底层具体实现不同
7. 放入有序表的东西，如果是基础类型，内部按值传递，内存占用就是这个东西的大小
8. 放入有序表的东西，如果不是基础类型，必须提供比较器，内部按引用传递，内存占用是这个东西内存地址的大小
9. 不管是什么底层具体实现，只要是有序表，都有以下固定的基本功能和固定的时间复杂度（O(N * logN)）

## 有序表的固定操作

1. voidput(Kkey,Vvalue)：将一个（key，value）记录加入到表中，或者将key的记录更新成value。
2. Vget(Kkey)：根据给定的key，查询value并返回。
3. voidremove(Kkey)：移除key的记录。
4. booleancontainsKey(Kkey)：询问是否有关于key的记录。
5. KfirstKey()：返回所有键值的排序结果中，最左（最小）的那个。
6. KlastKey()：返回所有键值的排序结果中，最右（最大）的那个。
7. KfloorKey(Kkey)：如果表中存入过key，返回key；否则返回所有键值的排序结果中，key的前一个。
8. KceilingKey(Kkey)：如果表中存入过key，返回key；否则返回所有键值的排序结果中，key的后一个。

以上所有操作时间复杂度都是O(logN)，N为有序表含有的记录数

## 题目整理

1. 反转单向和双向链表
2. 打印两个有序链表的公共部分
3. 判断一个链表是否为回文结构

+ 使用栈，将链表元素放入栈内，如果遇见相同的就弹出，最后栈为空则为回文结构

**代码**
```
using namespace std;
class PalindromeList {
public:
    bool chkPalindrome(ListNode* A) {
        stack <int> s;
        while (A){
        if (s.empty() || A->val != s.top()){
            s.push(A->val);
            A = A->next;
        }
        else {
            s.pop();
            A = A->next;
        }
        }
        return s.empty();
    }
};
```



