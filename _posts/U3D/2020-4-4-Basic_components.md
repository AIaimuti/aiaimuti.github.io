---
layout:     post
title:      基础组件
subtitle:   基础内容
date:       2020-4-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 入门
---

# 基本组件之摄像机
## 1.场景美化
### 1.给场景添加地板
1)新建“plane”物体作为地板；<br>
2)创建地板使用的材质球；<br>
3)编辑地板材质球，设置“Tiling”选项中的x，y 的值，使地板进行分块显示。<br>
Tiling：[ˈtaɪlɪŋ] 贴瓷砖，使贴图分块显示。

## 2.Game 视图
Game：游戏视图，游戏的预览（运行）窗口。<br>
当点击“播放”按钮，视图会自动切换到游戏视图进行预览；再一次的点击“播放”按钮，就可以退出游戏的运行状态，回归到编辑（Scene）视图。<br>
注意：游戏在运行状态时，做的任何操作都不会被保留。

## 3.摄像机常用操作
### 1.摄像机的简介与作用
简介：摄像机（Camera）摄像机就是我们眼睛，用于观察我们的游戏世界。<br>
眼睛有一个观察区间，叫做“视锥体”。所有在“视锥体”范围内的物体，我们都可以看到。<br>

作用：在合适的位置和角度观察我们的游戏世界。<br>
电影中的画面是由摄像机的角度和位置决定的；<br>
我们游戏中观看到的画面也是由摄像机的角度和位置决定的。

### 2.摄像机的基本操作
1)在Hierarchy面板上选中摄像机，Scene视图会出现预览窗口<br>
2)根据轴向移动摄像机的位置，旋转摄像机的角度。<br>
3)GameObject-->Align  With View(ctrl + Shift + F)对齐视图，让Scene与Game中观看角度和位置一致。<br>
4)创建摄像机：Hierarchy面板右键-->Camera。<br>

## 4.摄像机相关属性
## 1.Clear Flags[清除标记]
skybox:天空盒<br>
Solid Color：固定颜色（纯色）

## 2.Backgound[背景颜色]
当Clear Flags 为 Solid Color 时，场景的背景颜色。

## 3.Projection [投影]
Projective：透视模型，3D游戏使用。

## 4.Clipping Planes[切割面]
Near：近平面，摄像机最近能看到的东西。<br>
Far：远平面，摄像机能看到的最远的东西。

# 基本组件之灯光
## 1.灯光简介
灯光：Light，用于照亮我们的游戏世界。<br>
附加作用：烘托场景的氛围；使场景中产生阴影，增加真实感和立体感。<br>
在我们创建一个新的Scene场景时，场景中会默认带有两个游戏物体：一个是摄像机，一个是灯光。<br>
隐藏与显示游戏物体：物体Inspector面板上“图标”右侧的复选框。

## 1. 方向光
### 1.简介
方向光：Directional Light，用于模拟太阳，模拟自然光。<br>
方向光任何地方都能照射到，就和太阳一样，但是要注意照射方向。<br>

**创建方向光：**<br>
Hierarchy面板右键-->Light-->Directional Light

### 2.属性

**Type[类型]**<br>
用于切换灯光的类型。

**Color[颜色]**<br>
设置光的颜色

**Intensity[强度]**<br>
设置灯光的照射强度

**Shadow Type[阴影类型]**<br>
设置方向光照射到物体显示的投影效果。<br>
No Shadows：无阴影<br>
Hard Shadows：硬阴影<br>
Soft Shadows：软阴影

## 3.点光源
### 1.简介
点光源：Point Light，用于模拟电灯泡的照射效果。

**创建点光源：**<br>
Hierarchy面板右键-->Light-->Point Light

### 2.属性

**Range[范围]<br>**
设置点光源的照射范围，一个球状范围。<br>

## 4.聚光灯
### 1.简介
聚光灯：Spot Light，用于模拟聚光灯的照射效果。<br>

**创建聚光灯：**<br>
Hierarchy面板右键-->Light-->Spot Light

### 2.属性
**Spot Angle[聚光角]**<br>
设置聚光灯的照射角度


