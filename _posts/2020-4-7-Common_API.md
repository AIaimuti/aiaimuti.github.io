---
layout:     post
title:      常用API
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D 初级
---

# 常用API 之实例化与销毁
##1.实例化游戏物体
### 1.游戏中的案例介绍
在很多MMORPG 类的游戏中都有类似于“金钱副本”的副本关卡。在这类副本中通常都是限定一个时间，在这个时间内玩家可以尽情的破坏，然后收集金钱。<br>
分析游戏截图讲解场景元素：[见图]<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/Unity/game_instance.png)
1)场景中所有的坛子，模型都是一样的，坛子是预制体。<br>
2)坛子是从“天上”掉下来的，所以坛子模型身上有刚体组件。<br>
3)如果玩家在一段时间内没有打碎坛子，这些坛子会消失。

### 2.案例场景制作
1)创建地面模型；<br>
2)创建“坛子”模型，并制作成预制体；

### 3.实例化生成1 个坛子
GameObject.Instantiate（Object, Vector3, Quaternion）;
参数说明：
Object：用于实例化的预制体；<br>
Vector3：实例化后生成的物体所在的位置；<br>
Quaternion[四元数]：实例化后生成的物体的旋转状态；<br>
Quaternion.identity：无旋转；
Quaternion.Euler(45,45,45)：各轴偏移45度

### 4.构造随机位置
位置是用Vector3 类型进行表示的。<br>
X,Y,Z 三个值确定了物体在三维世界中的位置。<br>
Random.Range（min, max）：生成随机数。<br>
在min 和max 直接随机生成一个随机数。<br>
演示：按下键盘的一个键，就在随机位置实例化一个物体。

## 2.销毁游戏物体
GameObject.Destroy（Object, float）；定时销毁某个游戏物体。<br>
参数说明：<br>
Object：要销毁的游戏物体；<br>
float：时间，多少秒后销毁；

# 常用API 之Invoke 函数调用
## 1.金钱副本细节完善
### 1.宝箱自动掉落
给宝箱预制体添加刚体组件即可。

### 2.实现按键宝箱批量掉落
1)将实例化生成宝箱的代码单独封装成一个函数；<br>
2)使用for 循环，批量生成宝箱。

### 3.宝箱自动掉落
现在我们是通过按下某键，然后程序调用“生成宝箱”的函数，来实现功能。<br>
在真正的游戏中，这个“生成宝箱”的函数，也是需要由程序自动调用的。<br>
那么如何实现那？？

## 2.Invoke 函数
### 1.Invoke 函数代码
Invoke（string，float）：多少秒后执行某个函数[只会调用一次]。<br>
参数说明：<br>
String：要执行的函数的名称；<br>
Float：秒数，倒计时的时间；<br>
InvokeRepeating（string，float，float）：多少秒[第二个参数]后执行某个函数，并且以后每隔多少秒[第三个参数]都会执行该函数一次[重复调用N次]。<br>
参数说明：
String：要执行的函数的名称；<br>
Float：秒数，准备时间，预热时间；<br>
Float：秒数，重复调用的间隔时间；<br>
CancelInvoke()：取消这个脚本中所有的Invoke 调用。

### 2.Invoke 函数的家在哪儿？
Class 类可以理解成是一个“家”，各种各样的函数(方法)，字段，属性可以理解成是这个“家”的家庭成员。<br>
那么，这三个Invoke 相关的函数，他们的家在哪儿？<br>
回答：MonoBehaviour 类。<br>
我们先阶段写的脚本，都是默认继承“MonoBehaviour”类的，我们自己写的脚本类，都是这个“MonoBehaviour”类的子类，所以我们可以直接通过写方法名的形式，就可以调用父类中的方法。

## 3.金钱副本主角控制
### 1.主角基本控制
1)创建一个Cube 模型，美化一下，作为主角；<br>
2)添加刚体组件，使用刚体的MovePosition 结合按键控制主角移动。

### 2.控制主角与Box 碰撞
主角碰撞到Box，Box 自动销毁。

# 常用API 之消息发送
## 1.金钱副本细节完善
### 1.主角战斗
1)主角碰撞到Box，Box 消失后，在消失的位置生成1 枚金币；
2)金币设置为触发器模式，且自身要不停的旋转；
3)主角编写触发器处理代码，触发销毁金币。

## 2.SendMessage 消息发送
### 1.SendMessage 函数
gameObject.SendMessage（string）：通知这个游戏物体身上的脚本文件中的“指定方法”执行。
参数说明：
String：方法名，要执行的方法的名称；

### 2.完善金钱副本
1)创建一个GUIText 用于显示分数；
2)给金币创建脚本，金币获取到GUIText 组件的引用；
3)在金币脚本中编写“增加分数”的函数；
4)当金币销毁时，SendMessage 通知该“增加分数”函数执行。
