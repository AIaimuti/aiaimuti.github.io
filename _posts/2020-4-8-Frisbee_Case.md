---
layout:     post
title:      飞盘案例
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 初级
---

# 飞盘射击案例
## 1.飞盘射击游戏演示
试玩《飞盘射击》游戏成品Demo。

## 2.飞盘射击要点分析
### 1.UI 展示
游戏开始UI：游戏名称，游戏玩法，开始按钮。<br>
游戏进行UI：显示时间倒计时，显示当前分数。<br>
游戏结束UI：分数，返回按钮。

### 2.主角控制
持枪的一个手臂，枪口的朝向一直是朝向鼠标在屏幕上的位置。<br>
按住鼠标左键发射干掉目标。

### 3.AI 控制
飞盘在空中随机位置生成。<br>
当枪的射线碰撞到飞盘后，飞盘破碎掉落，分数加1 分。<br>

### 4.音效特效
游戏背景音乐，射击声。

## 3.飞盘射击项目准备
### 1.新建项目
使用Unity 创建一个新项目，并且建好必备的资源存储文件夹。

### 2.UnityPackage 包
Unity 资源的一种打包专用格式，类似于电脑上的RAR 压缩包，ZIP 压缩包。<br>
在网上搜索下载的Unity 的插件，模型，特效等资源大多数都是这个格式。

### 3.导入项目资源
两种导入方法：<br>
1)鼠标选中UnityPackage 资源包，直接拖拽到项目的Project 面板；<br>
2)在Project 面板Assets 文件夹中点击鼠标右键-->Import Package-->Custom Package... 浏览选择对应的资源导入。<br>
将《飞盘射击》必备的素材资源导入项目中。

### 4.项目场景基本设置
1)背景模型导入，调整灯光；<br>
2)手臂模型导入，调整位置；

# 飞盘射击案例之角色控制
## 1.鼠标控制手臂朝向
### 1.使用射线控制手臂朝向
主摄像机发射射线，与场景中的物体碰撞，获取一个碰撞点，然后手臂“朝向”这个点。<br>
Transform.LookAt(Vector3)：朝向(面向)世界中的一个点。

### 2.场景背景模型添加碰撞器组件
添加Mesh Collider 组件，使用简模进行碰撞检测。<br>
简模模型的作用：降低物理运算资源消耗，提高游戏性能；<br>
Vertices（verts）：顶点个数，Mesh 网格是由顶点组成面，面组成体。<br>
顶点个数越少，计算量越小。

### 3.控制手臂的旋转轴
手臂应该是以肩关节为轴进行旋转。<br>
和Unity 初级课程上部《疯狂教室》案例中旋转门的处理方式一样，新建父物体，通过父物体控制子物体。<br>

### 4.Debug 绘制测试线
枪口到碰撞点直接绘制一根测试线。<br>
Debug.DrawLine(Vector3, Vector3, Color)：绘制线<br>
和Debug.DrawRay（）用法一样。<br>
Transform.FindChild(string)：在当前物体的子物体中查找游戏物体。

## 2.枪口添加激光特效
### 1.枪口位置添加LineRenderer 组件
在枪口这个空游戏物体上添加LineRenderer 组件，设置相关属性。<br>
LineRenderer.SetPosition（int，Vector3）：设置线的位置。<br>
int：位置的索引值;<br>
Vector3：该位置所在的坐标值；

### 2.从其他项目中导出资源包
选中Project 面板中相应的资源，然后鼠标右键-->Export Package...
