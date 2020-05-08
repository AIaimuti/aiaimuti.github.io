---
layout:     post
title:      动画状态机的嵌套
subtitle:   基础内容
date:       2020-5-6
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
<iframe src="//player.bilibili.com/player.html?aid=540566111&bvid=BV1Ci4y1x7Rv&cid=187934977&page=1"  width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

状态机是UE4一个很好用的切换动画的工具，今天做的功能是在状态机里面再做一个状态机。<br>
实现的功能就是装备着武器时，是一个动画；<br>
没装备武器时，是另一个动画。

## 动画状态机的嵌套
先给出我们原始的状态机
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/State_Machine.png?raw=true)<br>
这个状态机分四个部分，站立/走/跑、冲刺、跳跃、着陆<br>
我们需要在站立/走/跑和冲刺的状态机中嵌套一个状态机，根据是否持有武器区分<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/State_Nesting.png?raw=true)<br>
其实核心内容就是这些，很简单<br>

## 遇到的问题
遇到了一个动画的问题<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Anima_Problem.png?raw=true)<br>
这个动画不是循环动画，向前走之后，动画结束时会回到初始位置；<br>
另外我导入了一个动画时持枪前进的，这个动画行走没有问题，但上半身是持枪状态；<br>
于是采用了Layered Blend Per Bone将两个动画混合，上半身用行走动画，下半身用持枪的动画；<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Layered_Blend_Per_Bone.png?raw=true)
### 具体步骤
双击Layered Blend Per Bone打开细节面板，在config选项中<br>
layer steup展开–—>Branch filter 加号添加新元素再展开<br>
输入spine_01(腰椎的骨骼)，Blend Depth选择1<br>
勾选Mesh Space Rotation Blend网格空间体选择混合 和 Mesh Space Scale Blend 网格空间体缩放混合<br>
相当于把人物切成两半，上半身用第一个动作，下半身用第二个动作<br>
最后，用红色按钮record录制我们的新动画，然后保存就可以以后直接用啦
