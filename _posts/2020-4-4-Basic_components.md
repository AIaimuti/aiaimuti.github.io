---
layout:     post
title:      基础组件
subtitle:   基础内容
date:       2020-4-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D
---

## 摄像机

摄像机（Camera）摄像机就是我们眼睛，用于观察我们的游戏世界。<br>

眼睛有一个观察区间，叫做“视锥体”。所有在“视锥体”范围内的物体我们都能看到。<br>

作用:在合适的位置和合适的角度观察我们的游戏世界。<br>

电影中的画面是由摄像机的角度和位置决定的；我们游戏中看到的画面也是摄像机的角度和位置决定的。

### 1.摄像机的基本操作

在Hierarchy面板上选中摄像机，Scene视图会出现预览窗口<br>

根据轴向移动摄像机的位置，旋转摄像机的角度。<br>

GameObject-->Align  With View(ctrl + Shift + F)对齐视图，让Scene与Game中观看角度和位置一致。<br>

创建摄像机：Hierarchy面板右键-->Camera。<br>

### 2.摄像机相关属性

**Clear Flags[清除标记]**<br>
skybox:天空盒<br>
Solid Color：固定颜色（纯色）

**Backgound[背景颜色]**<br>
当Clear Flags 为 Solid Color 时，场景的背景颜色。

**Projection [投影]**<br>
Projective：透视模型，3D游戏使用。

**Clipping Planes[切割面]**<br>
Near：近平面，摄像机最近能看到的东西。
Far：远平面，摄像机能看到的最远的东西。

## 灯光

灯光：Light，用于照亮我们的游戏世界。<br>

附加作用：烘托场景的氛围；使场景中产生阴影，增加真实感和立体感。<br>

在我们创建一个新的Scene场景时，场景中会默认带有两个游戏物体：一个是摄像机，一个是灯光。<br>

隐藏与显示游戏物体：物体Inspector面板上“图标”右侧的复选框。<br>

### 1. 方向光：Directional Light，用于模拟太阳，模拟自然光。<br>

方向光任何地方都能照射到，就像太阳一样，但是要注意照射方向。

**创建方向光：**<br>

Hierarchy面板右键-->Light-->Directional Light

**属性**

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

### 2.点光源：PointLight，用于模拟电灯泡的照射效果<br>

**创建点光源：**<br>
Hierarchy面板右键-->Light-->Point Light

**属性**<br>

**Range[范围]<br>**
设置点光源的照射范围，一个球状范围。<br>

### 3.聚光灯：Spot Light，用于模拟聚光灯的照射效果。<br>

**创建聚光灯：**<br>
Hierarchy面板右键-->Light-->Spot Light

**属性**<br>
Spot Angle[聚光角]<br>
设置聚光灯的照射角度


