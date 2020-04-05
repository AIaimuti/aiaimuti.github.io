---
layout:     post
title:      物理组件
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 入门
---

# 钢体

## 1.变换组件运动特点
使用Transform.Translate()方法移动物体的位置，特点如下：<br>
移动的物体会“穿透”场景中其他的物体模型；
移动的物体不会受重力影响（到达场景边缘外，不会下落）。

## 2.刚体组件简介

刚体：Rigidbody，属于物理类组件。
作用：添加了刚体组件的游戏物体，就有了重力，就会做自由落体运动。也就意
味着可以像现实中的物体一样运动。

给物体添加刚体组件<br>
选中游戏物体-->菜单Component-->Physics-->Rigidbody

## 3.刚体组件属性

**Mass[质量]**<br>
设置物体的质量，也就是重量。质量单位是KG。

**Drag[阻力]**<br>
空气阻力，0 表示无阻力，值很大时物体会停止运动。

**Angular Drag[角阻力]**<br>
受到扭曲力时的空气阻力，0 表示无阻力，值很大时物体会停止运动。

**Use Gravity[使用重力]**<br>
是否使用重力。

## 4.使用刚体移动物体
相关方法：
```
Rigidbody.MovePosition(Vector3)：使用刚体移动物体的位置。
使用刚体移动物体，物体是根据世界坐标系的方向移动的。
使用刚体移动物体，物体会触发物理相关的事件。
```
参数:
```
MovePosition 中的Vector3 要使用“当前位置”+ 方向的方式。
Transform.Position：属性当前物体的位置。
```
特点:<br>
使用刚体移动物体，特点如下：<br>
会于场景中的模型物体发生碰撞；<br>
会受重力影响（到达场景边缘外，会下落）。

# 碰撞体


使用刚体移动的物体，与场景中其他的物体相碰撞：<br>

其实是碰撞的目标物体的“碰撞体”组件，也就是Collider。<br>

另外和目标物体碰撞的，是我们移动的物体的自身的“碰撞体”组件。<br>

碰撞体可以理解为我们的模型的“外骨骼”。<br>

模型只要加了刚体，就必须要加碰撞体，否则没有意义。<br>

**给物体添加碰撞体组件**<br>

选中游戏物体-->菜单Component-->Physics-->Xxxx Collider<br>

（我们在Unity 中创建的基本模型，自身都带有碰撞体组件）

## 2.Box Collider

盒子碰撞体，形状是立方体形，用于包裹类似“立方体”的模型，比如：盒子，箱子，门，房子等。

**组件属性**

**Center[中心点]**<br>
设置Box Collider 的中心点。

**Size[大小]**<br>
设置Box Collider 的大小。

## 3.Sphere Collider

球形碰撞体，形状是球形，用于包裹类似“球形”的模型。

**组件属性**

**Radius[半径]**<br>
设置Sphere Collider 的大小。

## 4.Capsule Collider

胶囊碰撞体，形状是胶囊状，用于包裹“胶囊形”的模型。

**组件属性**

**Height[高度]**<br>
设置Capsule Collider 的高度。

**Direction[方向]**<br>
设置Capsule Collider 的高度方向（轴向）。

## 5.Mesh Collider

网格碰撞体，用于包裹复杂结构的模型。

**组件属性**

**Mesh[网格]**<br>
根据指定的网格，生成碰撞体。

# 刚体与碰撞体

 ![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/Unity/RigidBody_collider.png)
 
# 刚体常用方法介绍
 
## 1.AddForce()
 
给刚体添加一个力，让刚体按“世界坐标系”进行运动。
 
**代码：**
```
Rigidbody.AddForce(Vector3，ForceMode)；
Vector3：力的方向和大小；
ForceMode：力的模式[enum 类型]。
```

**ForceMode 参数**<br>
类型为枚举类型，以什么样的方式添加力给刚体。

枚举值说明<br>
Acceleration：（加速度）；<br>
Force：（力）这种模式通常用于设置真实的物理；<br>
Impulse：（冲击力）这种模式通常用于瞬间发生的力；<br>
VelocityChange：（速度的变化）；

## 2.AddRelativeForce()

给刚体添加一个力，让刚体按“自身坐标系”进行运动。

**代码**
```
Rigidbody.AddRelativeForce(Vector3，ForceMode)；
Vector3：力的方向和大小；
ForceMode：力的模式[enum 类型]。
```

## 3.FixedUpdate()

固定更新方法。<br>
所有和物理相关的操作，代码都要写在FixedUpdate（）方法体内。<br>
固定更新的时间间隔是0.02 秒，1 秒执行50 次。<br>
Edit-->Project Settings-->Time 面板中的Fixed Timestep 参数设置。

Update（）方法是每帧执行一次。<br>
画面每渲染完一次，就是一帧，每帧的时间是不固定的。<br>
在Update（）方法中执行物理操作，会出现卡顿的情况。

# 刚体碰撞事件监测与处理

## 1.碰撞事件简介

当一个用刚体控制的物体与另外一个物体碰撞时，就会触发碰撞事件。<br>
注：目标物体必须带有Collider 组件。<br>
碰撞Collision<br>
比如，一款射击类游戏，我们发射出了子弹，子弹是一个由刚体控制运动的物体，<br>
子弹射中了敌人，我们如何监测到这个碰撞？？

## 2.碰撞事件监测方法
```
OnCollisionEnter（Collision）//当碰撞开始时调用，只会调用该方法一次。
OnCollisionExit（Collision）//当碰撞结束时调用，只会调用该方法一次。
OnCollisionStay（Collision）//当碰撞进行中时，会持续调用该方法。
```
**Collision 参数**<br>
碰撞，一个类。作用：用于传递碰撞信息。<br>
Collision.gameObject 属性，与当前物体碰撞的物体的引用。<br>
gameObject.name 属性，当前物体的名字。

# 刚体触发事件监测与处理

## 1.触发事件简介

**触发器**
将碰撞体组件属性面板上的“Is Trigger”选项选中，当前的游戏物体的碰撞体就变成了触发器。<br>
移动的刚体物体会穿透碰撞体勾选了“Is Trigger”的物体。

**触发事件**
当一个用刚体控制的物体进入到另外一个物体的触发器范围内，就是触发事件。<br>
触发用途：不与目标物体发生直接的碰撞（接触），而是只要进入目标物体的“触发范围”就能执行某些特定操作。

## 2.触发事件监测方法

```
OnTriggerEnter（Collider）//当进入触发范围时开始时调用，只会调用该方法一次。
OnTriggerExit（Collider）//当离开触发范围时开始调用，只会调用该方法一次。
OnTriggerStay（Collider）//在触发范围内，会持续调用该方法。
```

**Collider 参数**<br>
碰撞体，一个类。作用：用于传递触发信息。<br>
Collider.gameObject 属性，进入触发范围内的目标物体的引用。<br>
gameObject.name 属性，当前物体的名字。

# 网格组件

## 1.网格过滤器组件

网格过滤器：Mesh Filter。<br>
该组件只有一个“Mesh”属性，用于设置当前游戏物体使用哪个模型进行展示。<br>
Mesh：网格，也就是模型。

## 2.网格渲染器组件

网格渲染器：Mesh Renderer。<br>
该组件用于“渲染”显示模型。如果没有该组件，模型就不会显示。

**属性**

**Cast Shadows [投射阴影]**<br>
On：开启阴影显示
Off：关闭阴影显示

**Receive Shadows [接收阴影]**<br>
选中就是接收
不选中就是不接收

**Materials [材质球]**<br>
用于设置用哪个材质球渲染当前的模型（Mesh）。<br>
我们拖拽到游戏物体身上的材质球，其实就是赋予给了这个组件的这个属性上。
