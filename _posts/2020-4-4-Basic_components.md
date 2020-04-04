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

1.摄像机的常用操作

+ 摄像机的简介与作用

简介：摄像机（Camera）摄像机就是我们眼睛，用于观察我们的游戏世界。
眼睛有一个观察区间，叫做“视锥体”。所有在“视锥体”范围内的物体我们都能看到。

+ 作用:在合适的位置和合适的角度观察我们的游戏世界。

电影中的画面是由摄像机的角度和位置决定的；我们游戏中看到的画面也是摄像机的角度和位置决定的。

+ 摄像机的基本操作

在Hierarchy面板上选中摄像机，Scene视图会出现预览窗口
根据轴向移动摄像机的位置，旋转摄像机的角度。
GameObject-->Align  With View(ctrl + Shift + F)对齐视图，让Scene与Game中观看角度和位置一致。
创建摄像机：Hierarchy面板右键-->Camera。

2. 摄像机相关属性

+ Clear Flags[清除标记]

skybox:天空盒
Solid Color：固定颜色（纯色）

+ Backgound[背景颜色]

当Clear Flags 为 Solid Color 时，场景的背景颜色。

+ Projection [投影]

Projective：透视模型，3D游戏使用。

+ Clipping Planes[切割面]

Near：近平面，摄像机最近能看到的东西。
Far：远平面，摄像机能看到的最远的东西。



