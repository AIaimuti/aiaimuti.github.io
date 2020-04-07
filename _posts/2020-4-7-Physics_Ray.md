---
layout:     post
title:      物理射线
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 初级
---
# 物理射线原理分析
## 1.物理射线简介
### 1.何为物理射线？
从一个点往一个方向，发射一根无限长的射线，这根射线与场景中的其余的游戏物体的碰撞体组件相碰撞，射线即结束。<br>
由于射线可以与物理组件Collider 相交互，所以“射线”也称之为“物理射线”。

### 2.物理射线的应用范围？
超人的激光眼，X 战警镭射眼。

## 2.物理射线相关方法
### 1.通过摄像机创建射线
演示：按下鼠标左键发射射线。<br>
**Camera.main**<br>
代表tag 设置为“MainCamera”的摄像机的Camera 组件的引用。

**m_Camera.ScreenPointToRay(Vector3)**<br>
摄像机组件(对象)下的一个方法。<br>
屏幕点转化为射线，这个方法会返回一个Ray 类型的射线。<br>
这个屏幕点通常写鼠标的点击位置，这样的话，就代表从摄像机的位置开始，往鼠标点击点这个方向，发射一根射线。

**Input.mousePosition**<br>
鼠标所在的位置值。

**Ray**<br>
射线，一个结构体。

**代码完整格式：**<br>
ray = Camera.main.ScreenPointToRay(Input.mousePosition);

### 2.检查射线与其他物体的碰撞
**RaycastHit**<br>
一个结构体，用于存储射线的碰撞信息。

**Physics.Raycast(Ray, out RaycastHit)**<br>
物理类下面有一个静态方法叫做Raycast()，射线检查。<br>
这个方法有16 个重载方式，我们现在使用第3 种重载方式。

第3 种重载：<br>
检查这根射线，如果射线与场景中的物理碰撞了，返回值为真，并且将碰撞信息存储到RaycastHit 类型的变量中。

**物理射线使用步骤**<br>
第一步：创建一根射线。<br>
第二步：检查这根射线与其他物体的碰撞，得到碰撞信息。<br>
第三步：通过碰撞信息对碰撞到的物体进行处理。
