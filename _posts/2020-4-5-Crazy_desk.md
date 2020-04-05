---
layout:     post
title:      疯狂教室案例开发
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 入门
---

# 疯狂教室案例开发

## 1.模型旋转实现开门效果

**1.模型中心点<br>**
模型身上的坐标轴的中心点，也就是我们模型的中心点。<br>
模型的的位置，旋转，缩放都是相对于模型的中心点来进行变化的。

**2.改变模型中心点**<br>
创建一个空物体，创建父子关系，通过父物体来控制子物体。<br>
也就间接的改变了模型的中心点。

**3.中心点工具**<br>
Center：当选中两个模型的时候，设置为“Center”，模型组的中心点就在两个模型的中间中心位置。<br>
Pivot：当选中两个模型的时候，设置为“Pivot”，模型组的中心点就在后选中的模型的中心点位置。

**4.使用键盘按键实现开关门**<br>

使用Transform.Rotate(Vector3，float)旋转模型：<br>
Vector3：沿某个轴向旋转<br>
Float：旋转的度数

## 2.触发器实现开关门

**1.添加触发器**<br>
创建一个空物体，添加“Box Collider”组件，并设置大小和中心点；<br>
将“Box Collider”勾选“Is Trigger”变成触发器；

**2.代码实现触发器开关门**<br>
OnTriggerEnter()<br>
OnTriggerExit()

**3.查找游戏物体**<br>
GameObject.Find(string)：[静态方法] 通过名字查找游戏物体<br>
String 游戏物体的名字

## 3.通过Tag 标签查找物体

**1.Tag 标签**<br>
标签可以起到标识，区分的作用。<br>
同一类的模型，我们可以根据需要给他们设置成统一的标签。<br>

**2.给模型添加Tag 标签**<br>
选中一个模型，在模型的Inspector 面板上的顶部位置，设置Tag 选项为一个具体的标签。<br>
如果说引擎提供的标签没有自己想要的标签，可以自己手动添加新标签。

**3.通过Tag 标签查找N个物体**<br>
GameObject.FindGameObjectsWithTag(string)：[静态方法]<br>
通过特定的标签，查找到所有“贴有”该标签的游戏物体，返回一个数组。<br>
String：标签名

**4.for 循环输出模型信息**<br>
通过for 循环遍历FindGameObjectsWithTag（）方法返回的数组，输出游戏物体的信息。

**5.通过键盘按键实现桌椅跳动**<br>
按下某键，桌椅全部上移2 米；<br>
抬起某键，桌椅全部下移2 米；

## 4.触发器实现桌椅跳动

**1.创建触发器**<br>
在桌椅的范围内，创建一个空物体，然后添加Box Collider，并调整大小。<br>
最后勾选“Is Trigger”属性。

**2.使用触发器实现桌椅跳动**<br>
OnTriggerEnter ()<br>
OnTriggerExit ()
