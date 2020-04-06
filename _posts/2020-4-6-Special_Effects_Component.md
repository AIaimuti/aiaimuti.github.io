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
设置拖尾显示的颜色。<br>
在实际开发中，美工人员给我们的贴图往往是“黑白图”，这种图片中黑色是透明区域，白色是不透明区域，可以简单记忆为“黑透白不透”。<br>
我们可以通过设置这里的Color属性值，来让白色区域显示成特定的颜色。

# 特效组件之LineRenderer
## 1.LineRenderer 简介
### 1.简介
LineRenderer，线渲染器，作用是用于渲染显示“线特效”。<br>
线：就是一根线。我们要想绘制一根线，需要至少两个点，在游戏中也不例外。<br>
现实生活中比如玩具激光灯照射出来的线。<br>

### 2.线特效在游戏中的运用
线特效在游戏中常用于渲染激光效果，或者用于子弹瞄准。

### 3.创建线特效
1)新建一个空游戏物体；<br>
2)给这个空游戏物体添加LineRenderer 组件，步骤如下：Component-->Effects-->Line Renderer

## 2.LineRenderer
### 1.Materials（材质球）
设置“线渲染器”这个组件使用哪个材质球来渲染线。

### 2.制作透明材质球
1)创建一个材质球；
2)设置该材质球的Shader 为Particles/Additive；（粒子/添加物）
3)给材质球拖拽赋予贴图；

## 3.LineRenderer 常用属性
### 1.Positions（位置）
设置线的开始点和结束点的位置。

### 2.Start Width（开始宽度）
设置线开始时的宽度。

### 3.End Width（结束宽度）
设置线结束时的宽度。

### 4.Start Color（开始颜色）
设置线的开始颜色。

### 5.End Color（结束颜色）
设置线的结束颜色。

# 声音组件之AudioSource
## 1.AudioClip 音频剪辑
### 1.音频简介
在Unity3D 游戏开发过程中，为了烘托场景氛围，会大量的使用到各种各样的“声音”来制造场景氛围。<br>
比如：游戏的背景声音，各种武器的特效声音，刀剑武器的挥舞声音......<br>
如果一个游戏中没有了声音，至少会降低玩家一半的游戏快感，声音在游戏开发和制作的过程中是非常重要的。

### 2.AudioClip
AudioClip：音频剪辑（音频片段）。<br>
我们导入到Unity 中的所有的声音文件，他们在Unity 引擎中的资源类型都是AudioClip 类型。<br>
Unity 能使用的音频格式： .aif .wav .mp3 .ogg<br>
我们可以在Project 面板的Assets 文件夹中创建一个“Audios”文件夹来存放和管理游戏中使用到的音频资源。

## 2.AudioSource 组件
### 1.组件简介
AudioSource：音频源组件，作用是用于播放音频剪辑（AudioClip）资源。<br>
你可以将音频源组件当成一个“音响”。

### 2.创建AudioSource
1)新建一个空游戏对象；
2)给这个空游戏对象添加AudioSource 组件，步骤如下：Component-->Audio-->AudioSource

### 3.AudioSource 常用属性
**<1>AudioClip（音频剪辑）**<br>
指定该音频源播放哪个音频文件。

**<2>Play On Awake（在唤醒时开始播放）**<br>
勾选之后，在游戏运行起来之后，就会开始播放。

**<3>Loop（循环）**<br>
勾选之后，声音会进入“单曲循环”状态。

**<4>Mute（静音）**<br>
勾选之后，静音，但是音频还是处于播放状态。

**<5>Volume（音量）**<br>
为0 时，无声音；为1 时，音量最大。

**<6>Spatial Blend（空间混合）**<br>
设置声音是2D 声音，还是3D 声音。
为0 时，是2D 声音；为1 时，是3D 声音。

### 4.Audio Listener 组件
Audio Listener：声音侦听器，其实就是我们在游戏世界中的“耳朵”。<br>
我们依靠这个组件来听游戏世界中的声音，如果没有了这个组件，我们是听不到任何声音的。<br>
默认状态这个组件，是挂载到我们的摄像机身上的。

### 5.AudioSource 常用函数
**<1>Play() 函数**<br>
播放音频剪辑。

**<2>Stop() 函数**<br>
停止播放。

**<3>Pause() 函数**<br>
暂停播放。
