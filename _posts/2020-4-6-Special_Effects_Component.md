---
layout:     post
title:      特效组件
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 初级
---

# 1.特效组件之TrailRenderer
## 1.TrailRenderer 简介
### 1.简介
TrailRenderer，拖尾渲染器，作用是用于渲染显示“拖尾特效”。<br>
拖尾：物体后面拖着的尾巴，现实生活中存在的拖尾比如流星拖尾。

### 2.拖尾在游戏中的运用
拖尾特效在游戏中也被大量的使用和运用，比如发射出去的炮弹，子弹，导弹。<br>
只要这些物体是高速运动的，为了体现他们的运动快，往往都会在他们的后面加上一个拖尾特效。<br>
这里可以看到拖尾的一个重要用途：体现物体的运动速度。

### 3.创建拖尾特效
1)新建一个空游戏物体；<br>
2)给这个空游戏物体添加TrailRenderer 组件，步骤如下：Component-->Effects-->Trail Renderer<br>
3)在Scene 面板移动这个空物体的位置，就可以看到最原始的拖尾效果。

## 2.TrailRenderer 材质球
### 1.Materials（材质球）
设置“拖尾渲染器”这个组件使用哪个材质球来渲染拖尾。

### 2.注意事项：
Unity3D 中所有以“Renderer”为后缀进行命名的组件，都需要给他们指定材质球，比如之前讲解的Mesh Renderer。<br>
当这类组件身上没有材质球的时候，默认就会显示成“粉红色”。以后再看到某些游戏物体显示成了这种“粉红色”，你就要知道这些游戏物体缺失材质球。<br>

### 3.制作透明材质球
1)创建一个材质球；<br>
2)设置该材质球的Shader 为Particles/Additive；（粒子/添加物）<br>
3)给材质球拖拽赋予贴图；

## 3.TrailRenderer 常用属性
### 1.Time（时间）
设置拖尾特效的持续时间。

### 2.Start Width（开始宽度）
设置拖尾开始时的宽度。

### 3.End Width（结束宽度）
设置拖尾结束时的宽度。

### 4.Color（颜色）
设置拖尾显示的颜色。
在实际开发中，美工人员给我们的贴图往往是“黑白图”，这种图片中黑色是透明区域，白色是不透明区域，可以简单记忆为“黑透白不透”。
我们可以通过设置这里的Color属性值，来让白色区域显示成特定的颜色。
