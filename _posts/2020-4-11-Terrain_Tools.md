---
layout:     post
title:      UE4 地形工具
subtitle:   基础内容
date:       2020-4-11
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 
---
# UE4 地形工具
## 创建参数
材质
section Size：每个正方形里的小格子数量
number of Com：正方形格子的数量 长*宽

## 雕刻
雕刻就是更改地形的定点。
左键按住：地形隆起，
Shift + 左键按住：地形凹陷
雕刻工具：各类地形

Tool Setting：
Tool  Strength ：地形变化强度
Brush Size：鼠标圆圈的范围
Brush Fallof：衰减系数，数值越低，鼠标圆圈范围内影响地形的点就越多

## 绘制：
根据需求进行地形的分层

## 管理：
管理处可以对地形进行添加或删除
可以观察分辨率是否符合游戏的要求

编辑样条曲线：
Ctrl + 左键作为起始点，按住不放继续添加点
可以制作道路、河流。
起始点和结束点汇合就会形成圆。
点击一个面-->分段：就会选中所有面
细节-->Landscape Spline Meshes-->样条网格体-->点+号添加网格模型
下方参数可以调整模型的大小和轴向、添加材质

当地形与样条中间有空隙时
编辑工具样条-->所有样条:可以使地形适应样条
光源改成可移动，则时刻在渲染阴影
在地形编辑模式无法修改物体属性，只有切换到放置才能修改


