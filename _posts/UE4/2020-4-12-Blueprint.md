---
layout:     post
title:      UE4 蓝图
subtitle:   基础内容
date:       2020-4-12
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 入门系列
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

字体导入<br>
1）右键-->导入文件 Import Asserts-->添加字体<br>
2）桌面拖拽到Content Browser内容管理器

## 电梯
### lerp
A：数值<br>
B：数值<br>
Alpha：0~1之间<br>
在Alpha从0变道1时，lerp会输出A到B之间的数值，这个数值随着Alpha的变化而变化

### 设置相对位置
SetActorLocation：设置物体相对与整个Actor蓝图类的位置。<br>
SetRelativeLocation：设置物体相对于Actor蓝图类的某个组件的位置。<br>
SetWorldLocation：设置物体相对于世界坐标的位置。

### 双向自动门
物体-->蓝点拖拽-->GetRelativeLocation，可以直接获取物体的相对位置，避免了一直查看细节面板<br>
设置变量保存当前值，再将改当前值的偏移量保存，赋值给lerp，就可以实现完全程序化操作，避免，避免了一直查看细节面板

# set设置变量要放在进入触发器的时候，避免按键和鼠标重复获取获取当前值

### 拾取钥匙开门
Alt + 变量：设置变量当前值<br>
Ctrl + 变量：获取变量当前值

PrintString：左上角输出语句<br>
Branch:分流 与Boolean布尔型数值相配合使用 快捷键B + 鼠标左键

Get All Actors Of Class：获取场景中指定的实例，输出数组<br>
可以进行场景Actor间的蓝图通信

## 触发加速
触发加速即小白人触发后移动速度增加，顾名思义，第一步要先与小白人建立通信
### 类型转换
Cast To ThirdPersonCharacter：<br>
1)蓝图通信，转化到指定的的类型访问其中变量，一般与触发盒子配合使用，想和谁通信就转换为谁的类型<br>
2)针对性：仅转化后的类型可以访问触发该Actor蓝图类的功能
自定义事件：
Custome Events：可以自定义功能

## 变量引用
采用类型转换时，引用引脚拖拽出很多线会导致蓝图界面很乱。因此采用变量引用来方法来用一个变量来表示类型转换的引脚

IsValid：用带？的IsValid工具，变量引用后要判断变量是否有效，有效才可以继续执行。

EventTick：每一帧执行操作

变量引用类似全局变量，需要打开变量右边的眼睛，且编译一下，才能在所属物体的默认里看见<br>
点击默认旁边的小吸管可以选取指定物体，此方法只能单独引用该类的一个物体<br>
如果想引用该类的所有物体，则需要Get All Actors Of Class + For Each Loop<br>
且Get All Actors Of Class必须先选择再获得。

For Each Loop:<br>
快捷键F + 鼠标左键<br>
Exec：输入<br>
Array：该类下多个物体的实例<br>
Loop Body：执行输出<br>
Array Element：输出的被执行事件的Array<br>
Array Index：当前循环到哪个物体，输出数值<br>
Completed：循环完了执行

## 做个小游戏

### 新的蓝图类

执行控制台命令<br>
Execute Console Command<br>
控制台输入ce + 空格 + 关卡中蓝图自定义事件的名字<br>
Actor蓝图与关卡蓝图通信

For Each Loop With Break：<br>
在For Each Loop中加入了Break，可以中断循环<br>

Set Visibity：设置可视化组件

TextRender：文本可视化组件

Alt + 鼠标左键 去掉某个线<br>
Ctrl + 鼠标左键 更改某个线

点击变量后面的小方快可以改变变量的数值类型

### Spot Light
intensity：强度<br>
Lihgt Color：颜色<br>
Attenuation radius：衰减半径，即光照射距离调整<br>
Inner Cone Angle：内锥角，内部锥形光源<br>
Outer Cone Angle: 外锥角，外部锥形光源<br>
Temperature：色温<br>
Affects World：是否影响周围环境，即是否打开<br>
Cast Shadows：是否使用阴影

## 判断机关是否可以开启逻辑
我们所需逻辑是当所有灯被点亮，开启机关，<br>
For Each Loop With Break相当于检测每个灯是否打开，<br>
若打开，则开启机关变量为true<br>
若关闭，则开启机关变量为false，break结束循环<br>
若循环能完成，则说明开启机关变量为全为true，通信关卡蓝图，ce + 关卡蓝图中自定义事件名字，执行机关命令

左键选中物体，在蓝图中可以右键创建引用

## 蓝图接口
如果这个蓝图里面有对应的蓝图接口，发送的消息才会产生通信。<br>
蓝图接口如果添加输入输出就不能作为事件使用，但是没有添加输入输出的也可当作函数使用

蓝图接口可用于蓝图通信，步骤如下：<br>
1）创建蓝图接口，内容管理器右键-->高级功能蓝图类-->创建蓝图接口<br>
2）在需要使用的蓝图类中类管理中添加所创建的蓝图接口，并以此接口事件开始蓝图功能。<br>
3）在发送信息的类里创建通信所需变量。<br>
4）在需要使用的蓝图类中并将自己赋值给发送信息类中的创建的通信所需变量。<br>
5）发送信息的类中判断是否变量中有值，如果有，则发送信息，开始蓝图接口事件。

## 拾取物品_射线检测
LineTraceByChannel：射线检测<br>
BoxTraceByChannel：盒体检测<br>
SphereTraceByChannel：球体检测<br>
CapsuleTraceByChannel：胶囊体检测

制作射线的思想：需要一个起始点和一个结束点构成线段，通过检测结束点是否发生碰撞来达到检测效果

初始点：<br>
摄像机FirstPersonCamera，我们将Camera初始点的x，y，z置0，相当于坐标的校正，更符合人物在3D场景中的移动特性。

结束点：<br>
GetForwardVector：可以以获取Character面向的方向，并以（0，1）的三维坐标坐标作为输出<br>
既然已知方向，那么从我们方向向前增加一段距离，不久相当于射线检测的功能了，增加距离即将输出乘以一个值，如500，<br>
可以想象就是我们人拿了一个棍子，在往我们面向的方向探测，但这仅有方向信息，没有位置信息，<br>
因此，我们将探测点的坐标加上Camera的坐标，就既有位置信息又有方向信息了<br>

实际应用时，射线检测点和我们的准星稍微有些差距，通过在结束点的z轴方向增加一点偏移就可以更完美了
如果想让我们的检测的目标做出回应，我们要设置自己的追踪通道，并将物体相应的响应通道打开。
Setting-->ProjectSetting-->Collision--NewTraceChannel 
设置-->项目设置-->碰撞-->创建通道 并将Defult Response 设置为ignore


先选中某个物体，然后再到蓝图里添加，在AddConponent中会直接显示
custom自定义

BreakHitResult：会将射线碰撞结果相关属性进行拆分，形成多个信息引脚

骨架网格体设置碰撞：
右键Creat-->PhysicsAssert-->Create,会出现带黄色横线的图标为骨架网格体的物理资产，然后需要在紫色骨架网格体界面添加物理资产
骨架网格体和静态网格体的区别：
1）紫色的是骨架网格体Skeletal Mesh，蓝色的是静态网格体Static Mesh
2）骨架网格体可以播放动画，静态网格体不可以
