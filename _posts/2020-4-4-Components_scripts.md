---
layout:     post
title:      组件与脚本
subtitle:   基础内容
date:       2020-4-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D
---

# 组件

组件：Component，游戏物体的组成零件。<br>

Unity 3D就是一款“组件式”游戏开发引擎，使用各种各样的组件拼装出我们的游戏物体，最终拼接出一款完整的游戏。

## Transform组件
Transform：变换。所有游戏物体都具备的一个组件，也是最基础的一个组件，用于存储物体的基本信息。<br>
Position：位置；<br>
Rotation：选择；<br>
Scale：缩放；<br>

组件的启用和关闭：
点击Inspector面板上相应的组件图标右侧复选项，来进行切换

# C#脚本

## 1.何为脚本？

脚本：Scrpit，用于控制游戏的逻辑。<br>
Unity 3D 主流开发语言为C#

## 2.管理脚本

在Assets文件夹中创建“Scrpits”文件夹，管理脚本资源。

## 3.创建脚本

在Assets文件夹上右键-->Create-->C# Scrpit，马上改名。<br>
C# 脚本文件的后缀是“.cs”。<br>
双击脚本文件，就可以调用出“代码编辑器”进行代码的展示。

## 4.脚本代码简介

Start()方法：当游戏运行起来，就会马上执行，且执行一次。<br>
Update()方法：循环调用，每帧调用一次。一秒钟大概执行60次。<br>
帧:我们的游戏画面是在不停的刷新，每刷新一次，就是一帧。<br>
输出调试;Debug.log();

Start()和update()方法都是Unity 3D内部的“事件方法”，不需要我们人工调用，系统会自动调用和管理这些“事件方法”。

## 5.使用脚本

直接将脚本拖拽到Hierarchy面板上的物体上；<br>
直接将脚本拖拽到物体的Inspector面板上；<br>
运行游戏，脚本就会执行

## 6.Console面板

Console：控制台。用于输出显示游戏运行过程的调试信息。

**功能按钮：**
Clear：清除功能，清除控制台中的信息；<br>
Collapase：折叠功能，将相同的内容合并到一条显示，更新后面的数字；<br>
Clear on play:运行时清除之前的内容。

# 键盘鼠标操作

## 1.获取键盘输入

相关代码
```
Input.Getkey(); //按下某键后，持续返回true
Input.GetkeyDown();  //按下某键的一瞬间，返回true
Input.GetkeyUp();  //抬起某键的一瞬间，返回true
```
返回值：bool类型<br>
参数：KeyCode枚举（Enum）<br>
KeyCode:键码，保存了物理按键“索引码”。

## 2.获取鼠标输入

相关代码：
```
Input.GetMouseButton(); //按下某键后，持续返回true
Input.GetMouseButtonDown(); //按下某键的一瞬间，返回true
Input.GetMouseButtonUp();  //抬起某键的一瞬间，返回true
```
返回值：bool类型<br>
参数：鼠标按键索引值，0->左键，1->右键,2->中键

# 使用变换组件移动游戏物体

**游戏物体与组件关系图如下：**
 ![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/Unity/Game_Object.jpg)
 
所有的组件都是类：当组件挂载到物体身上后运行，引擎会自动实例化对象；我们写的脚本类是游戏物体的大脑。

 ## 1.变换组件移动物体
 
相关方法：
```
gameObject.GetComponent<T>(); //获取相应组件的引用,查找当前游戏物体上的某个组件，然后保存它的引用。
Tramsform.Translate(Vector3,Space); 移动物体的位置,游戏物体往某个方向移动；以自身坐标系或世界坐标系。
```
## 2. 相关参数
**Vector3[struct]:三维向量<br>**
向量，可以表示一个方向，也可以表示一个位置。

**Space[enum]：空间<br>**
Space.Self：表示物体自身的坐标系<br>
Space.World：表示物体所在世界的坐标系

# 键盘控制移动方向
使用键盘上的“W,A,S,D”来控制物体的前后左右移动。
