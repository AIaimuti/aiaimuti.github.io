---
layout:     post
title:      UE4 基础概念
subtitle:   基础内容
date:       2020-4-12
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 入门系列
---

# UE4 基础概念
## 快捷键
Edit-->Editor Preferences-->General-->Keyboard shortcuts<br>
编辑-->编辑器偏好设置-->通用-->快捷键

Alt + L：隐藏地形<br><br>
Alt + C：显示所有碰撞<br>
Alt + F：显示雾<br>
T：是否能选中半透明物体<br>
Ctrl + P：打开内容浏览器<br>
Ctrl + Alt + 鼠标左键：框选<br>
H/Ctrl + H：隐藏/显示选中物体<br>
鼠标中键：测距（仅正交模式）<br>
Ctrl + E：快速编辑物体<br>
Ctrl + B：打开物体路径<br>
Ctrl + G：打组<br>
Shift + G：取消打组<br>
Alt + P：开始游戏<br>
Alt + S：模拟运行，上帝视角<br>
L + 鼠标左键：快速生成点光源

## 蓝图快捷键汇总
G + 左键：Gate节点<br>
M + 左键：MutiBate节点<br>
F + 左键：ForEachLoop节点<br>
O + 节点：DoOnce节点<br>
N + 左键：DoN节点<br>
B + 左键：分支节点<br>
S + 左键：顺序节点<br>
D + 左键：延迟节点<br>
C：注释

## Tips
帧，FPS，图形浏览器每秒刷新的次数<br>
时间轴的Update和EventTick都是以帧为单位实时更新的

编译，新建的变量如果不编译直接在另一个蓝图使用会出现黄色的提示

对场景中物体属性的更改不会影响本体，想应用于所有场景需要打开物体编辑器

选择场景中蓝图类的时候不要双击，双击后变成选中蓝图类中的组件

鼠标侧键是返回的快捷键

# 蓝图基础概念

## 函数：
在程序设计中，常将一些常用的功能编写成函数，放在函数库中供公共使用，要善于利用函数，以减少写程序的工作量。
普通函数有执行引脚，纯函数没有执行引脚，只有输出值

## 宏和函数的区别
1）宏只在当前蓝图类中有效，其它蓝图类中无法看见，函数则可以，除非是宏库中的宏，宏库可以右键添加
2）宏可以自定义输入输出引脚的个数，而函数不能修改。

## 事件和函数的区别
1）事件是红色，函数时蓝色
2）事件可以延时，函数不可以
3）函数有返回值，事件没有   
4）事件可以作为回调函数，函数不可以
5）事件可以作为输入事件和碰撞事件的处理
6）实现蓝图接口时，有返回值的变成函数，返回值的变成事件
7）事件可以用来发送网络消息，而函数不行。

## 变量
UE4中的变量Set就是声明
Get就是使用
也可以手动设置变量的默认值

## 面向对象思想
封装、继承、多态
封装：即隐藏功能的实现，只提供对外的接口
继承：即子类继承父类的属性
多态：子类的延伸可以多种多样

类和对象：类是对象的抽象，对象是类的实例化。类是抽象的，不用占用内存，而对象是具体的，占储存空间。类是创建对象的蓝图，它是一个定义包括特定类型的的对象的和变量的软件模版

## object、actor、pawn、Character、component
继承、包含关系
Object>actor>pawn>Character
component是包含在actor中的

## 控制权、玩家输入
在Pawn和Character中是默认支撑玩家输入的，但在Actor中是需要额外开启的。
这也和他们的控制权有关系，pawn和character都能被角色控制，拥有控制权，但是actor不行。


## Actor和关卡蓝图的关系
Actor>level Blueprint actor，关卡蓝图也是一个actor，并且是actor的子类


