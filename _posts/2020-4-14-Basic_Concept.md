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
## 宏


