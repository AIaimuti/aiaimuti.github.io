---
layout:     post
title:      UE4 蓝图
subtitle:   基础内容
date:       2020-4-12
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 
---


## 自动门
Actor蓝图类<br>
设置碰撞体、触发开始/结束、时间轴、门组件、相对旋转
### 触发器
放置-->基本-->盒体触发器<br>
OnComponentBeginOverlap：当物体开始重叠时，即触发开始时<br>
OnComponentEndOverlap：当物体结束重叠时，即触发结束时

### 时间轴
Play：时间轴开始正向移动。<br>
Play From Start：从时间轴的第一帧开始正向移动。<br>
Stop：停止时间轴移动。<br>
Reverse：时间轴开始反向移动。<br>
Reverse From End：从时间轴的最后一帧开始反向移动。<br>
Set Now Time：设置时间轴开始的时间。<br>
Update：时刻更新物体的每一帧。<br>
Finshed：结束时开始触发。<br>
新建轨迹：输出时间轴轨迹

双击打开时间轴：<br>
Add Float Track：添加浮点型轨迹<br>
右键添加关键帧，左上角设置数值，且能按数值以及时间范围进行水平和垂直的适应<br>
length ：设置时间长度，一定与帧数持续时间一致<br>
右键原点：auto对曲线平滑

三个旋转：<br>
SetActorRotation：设置物体相对与整个Actor蓝图类旋转。<br>
SetRelativeRotation：设置物体相对于Actor蓝图类的某个组件旋转。<br>
SetWorldRotation：设置物体相对于世界坐标旋转。

右键 New Rotation-->Split Sturt Pin：展开X、Y、Z输入点

## 按E开关门
获取玩家控制器、按键E、开始/禁用输入、Gate流程控制、Filp Flop触发器、时间轴、门组件、相对旋转
### 获取玩家控制器
GetPlayerController：获取玩家控制权

### 开始/禁用输入
EnableInput：开始输入<br>
DisableInput：禁用输入

GetPlayerController.return Value-->EnableInput.PlayerController

### Gate流程控制
快捷键G + 鼠标左键<br>
Enter：开始流程<br>
Open：打开流程控制器<br>
Close：关闭流程控制器<br>
Toggle：表示切换门的状态,如果门是开的,则关上；如果门是关的,则开上

### Filp Flop触发器
重复执行A、B，执行完A，下次触发执行B，反复循环

## 鼠标点击开关门
获取玩家控制器、鼠标点击、开始/禁用输入、Gate流程控制、Filp Flop触发器、时间轴、门组件、相对旋转<br>
Door-->OnClick 鼠标点击触发<br>
世界编辑器World Outliner -->游戏模式 Game Mode-->玩家控制器类新建玩家-->details-->Mouse interFace-->Enable Click Events
