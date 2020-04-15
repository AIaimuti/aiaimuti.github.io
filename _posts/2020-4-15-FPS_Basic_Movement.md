---
layout:     post
title:      Fps游戏准备阶段
subtitle:   基础内容
date:       2020-4-15
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 蓝图实现FPS系列
---

## 材质编辑界面
数字1，2，3，4 + 鼠标左键分别对应的是1234维的常数节点，<br>
将材质选中 然后点击材质框旁边的小箭头可以快速应用<br>
1维常数的参数快捷键是S，<br>
三维常数的参数快捷键是V。

材质保存需要很长时间，但是进行材质实例的保存很快<br>

## 小白人骨骼插槽：
以添加摄像机为例<br>
编辑界面双击骨架网格体，找到head，右键点击add Socket添加插槽<br>
然后右键点击新创建的插槽，Add Preview Asserts添加预览资源选择摄像机<br>
如果视图选项中没有摄像机，在Add Preview Asserts右下角View Options，选择Show Engine Asserts<br>

在动画中调整摄像机<br>
编辑界面上方选择Preview Animation，选择指定的动画预览，再进行调整
调整好后，回到编辑界面，选择摄像机，在细节面板插槽中选择刚才创建的插槽。
然后再细节面板的Camera Options中勾选Use Pawn Control Rotation
将Camera固定在Pawn上，


## 添加小白人运动操作
编辑界面菜单栏edit-->Project Setting-->Engine-->Axis Mappings<br>
WS：控制前后，灵敏度W为1，S为-1<br>
AD：控制左右，灵敏度A为-1，D为1<br>
UP：控制视角，鼠标Y，灵敏度为-0.3，鼠标和我们视角是相反的，<br>
LR：控制左右，鼠标X，灵敏度为0.3
上述为轴映射，需要一直按才有效。

在一个普通的三维空间里，最简单的移动就是直接修改角色的坐标。所以，我们的角色只要有一个包含坐标信息的组件，就可以通过基本的移动组件完成移动。<br><br>
我们的Capsule Component胶囊体组件是一个根组件，它移动，它的相关组件都会移动，因此我们只需要改变他在实际中的坐标即可。<br>
Capsule Component胶囊体组件获取在世界中的旋转GetWorldRotation，实际上是获取在X、Y平面上的坐标；<br>
WS和AD都是在X、Y平面上移动，因此使用AddMovementInput

AddMovementInput这个函数会根据第一个参数World Direction的值去移动角色，<br>
第二个参数Scale Value是个浮点数，如果这个数是 1 的话，那么它会按照第一个参数的方向去添加，<br>
如果第二个参数是-1的话，那么会往第一个参数的反方向去添加。

GetActorForwardVector是获取在世界空间中CharactorX轴上的向量，和世界前方一致返回1，和世界后方一致返回-1<br>
GetActorRightVector是获取在世界空间中CharactorY轴上的向量，和世界左方一致返回-1，和世界右方一致返回1

然后是鼠标控制方向，
Y轴方向用AddControllerPitchInput，Pitch表示点头就是绕 Y 移动；
Z轴方向用AddControllerYawInput，Yaw表示摇头 就是绕 Z 移动；


## 添加小白人加速、跳跃
编辑界面菜单栏edit-->Project Setting-->Engine-->Action Mappings<br>
Run:Lift Shift和Right Shift
Jump:Space Bar
上述映射为动作映射，按下就生效

Run需要Character Movement组件，
Pressed-->Set Max Walk Speed 500
Released-->Set Max Walk Speed 200

Jump：
Pressed-->Jump
Released-->Stop Jumping







