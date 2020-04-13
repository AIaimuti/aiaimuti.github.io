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

Box Collision：盒碰撞体
Sphere Collision：球形碰撞体

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

字体导入
1）右键-->导入文件 Import Asserts-->添加字体
2）桌面拖拽到Content Browser内容管理器

## 电梯
### lerp
A：数值
B：数值
Alpha：0~1之间
在Alpha从0变道1时，lerp会输出A到B之间的数值，这个数值随着Alpha的变化而变化

### 设置相对位置
SetActorLocation：设置物体相对与整个Actor蓝图类的位置。<br>
SetRelativeLocation：设置物体相对于Actor蓝图类的某个组件的位置。<br>
SetWorldLocation：设置物体相对于世界坐标的位置。

### 双向自动门
物体-->蓝点拖拽-->GetRelativeLocation，可以直接获取物体的相对位置，避免了一直查看细节面板
设置变量保存当前值，再将改当前值的偏移量保存，赋值给lerp，就可以实现完全程序化操作，避免，避免了一直查看细节面板

# set设置变量要放在进入触发器的时候，避免按键和鼠标重复获取获取当前值

### 拾取钥匙开门
Alt + 变量：设置变量当前值
Ctrl + 变量：获取变量当前值

PrintString：左上角输出语句
Branch:分流 与Boolean布尔型数值相配合使用

Get All Actors Of Class：获取场景中指定的实例，输出数组
可以进行场景Actor间的蓝图通信

## 触发加速
触发加速即小白人触发后移动速度增加，顾名思义，第一步要先与小白人建立通信
### 类型转换
Cast To ThirdPersonCharacter：
1)蓝图通信，转化到指定的的类型访问其中变量，一般与触发盒子配合使用，想和谁通信就转换为谁的类型
2)针对性：仅转化后的类型可以访问触发该Actor蓝图类的功能
自定义事件：
Custome Events：可以自定义功能

## 变量引用
采用类型转换时，引用引脚拖拽出很多线会导致蓝图界面很乱。因此采用变量引用来方法来用一个变量来表示类型转换的引脚

