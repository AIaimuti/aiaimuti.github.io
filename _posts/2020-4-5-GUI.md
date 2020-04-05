---
layout:     post
title:      旧版UI组件
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 初级
---

# 旧版UI组件
## UI 简介
### 1.什么是UI？
UI 就是用户操作界面。<br>
在使用Unity 开发游戏（MMORPG，MMOARPG）的客户端的时候，至少50%的工作量是在写UI 界面和UI 界面的逻辑。<br>
Unity 刚入行的新人，在公司中主要干的也是编写UI 界面和UI 界面逻辑。

### 2.常用UI 系统简介
**<1>NGUI**<br>
NGUI 是一款使用最多的第三方的UI 插件。<br>
目前国内大部分的游戏的界面UI，都是使用NGUI 这个插件来编写实现的。

**<2>UGUI**<br>
Unity 自4.6 版本后自带的一套UI 系统。<br>
随着UGUI 的不断完善，慢慢的也有公司使用UGUI 来编写游戏的界面UI。<br>
GameObject-->UI 菜单下，是UGUI 的相关UI 对象。<br>
Component --> UI 菜单下，是UGUI 的相关UI 组件。

**<3>OnGUI**<br>
现在主要用于Unity 引擎的界面扩展。<br>
NGUI 和UGUI 都是“所见即所得”的形式制作UI，而OnGUI 类似于Web开发中的html 和css 的编写。

**<4>Legacy GUI**<br>
旧版UI，只有两个组件,文字和图片,配合鼠标事件来实现界面UI。<br>
这个在公司的实际游戏项目开发中，几乎不会用到。<br>
但是我们要讲这个，也要学习这个，因为这个UI 系统非常简单，在我们学习Unity 的过程中，可以实现一些“并不是很漂亮，但能用”的UI 界面。

## 2.GUI Text 组件
### 1.GUIText 简介
GUIText 组件主要用于文字的显示。

### 2.创建GUIText
1)新建一个空游戏物体。<br>
2)给这个空游戏物体添加“GUIText”组件，步骤如下：Component->Rendering->GUI Text<br>
3)这个游戏物体就可以用来负责显示文字了。

### 3.GUIText 常用属性
**Text（文字）**<br>
设置GUIText 组件显示的文字。

**Font Size（文字大小）**<br>
设置文字显示的大小，默认是0。

**Pixel Offset（像素位置偏移）**<br>
通过x 轴和y 轴两个值，设置文本组件在场景中显示的位置。

要点注意<br>
前面三个属性设置完毕后，我们的文本UI 就可以正常显示了。<br>
如果文字还是显示不出，应该在添加组件中Renderring中添加GUILayer层<br>
另外GUIText 组件只能在Game 窗口测试，Scene 窗口看不到。

**Color（颜色）**<br>
用于设置文字的显示颜色。

## 4.GUI Texture 组件
### 1.GUITexture 简介
GUITexture 组件主要用于图片的显示。

### 2.创建GUITexture
1)新建一个空游戏物体。<br>
2)给这个空游戏物体添加“GUITexture”组件，步骤如下：Component->Rendering->GUI Texture<br>
3)这个游戏物体就可以用来负责显示图片了。

### 3.GUI Texture 使用步骤
1)首先先将GUITexture 的Scale 缩放属性全部设置为0.1 的显示比例。<br>
2)然后再设置GUITexture 组件的相关属性。

### 3.GUI Texture 常用属性
**Texture（纹理）**<br>
设置要显示的图片。

**Color（颜色）**<br>
设置图片的颜色。默认状态时Color 属性是不会影响图片显示效果的。

**Pixel Inset（像素设置）**<br>
X（轴）和Y（轴）设置图片显示的位置。<br>
W（width）和H（height）设置图片的宽度和高度。

要点注意
GUITexture 组件同样也只能在Game 窗口测试，Scene 窗口看不到。

## 鼠标事件
### 1.简介
之前讲解的Input 类下面的鼠标输入是全局的，且只能获取鼠标的按键状态。<br>
而今天要讲解的“鼠标事件”是挂载到某一个游戏物体身上，且只有当我们的鼠标操作该游戏物体时，对应的鼠标事件才会生效。

### 2.常用事件方法
OnMouseEnter（） ：鼠标进入
OnMouseExit（） ：鼠标离开
OnMouseDown（） ：鼠标按下[单击]

### 3.颜色参数
Color 结构体，里面有常用的颜色。
Color.red；Color.green；Color.blue ......
