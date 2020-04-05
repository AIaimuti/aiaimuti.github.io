---
layout:     post
title:      PC端游戏打包与发布
subtitle:   基础内容
date:       2020-4-5
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - Unity 3D
---
# PC 端游戏打包与发布

## 1.游戏打包发布简介

**1.简介**<br>

现在的项目文件必须在Unity 引擎中才能运行，通过“打包发布”可以将工程文件转换成独立的“游戏文件”，就可以脱离Unity 引擎直接在电脑上运行。<br>
打包好的“游戏文件”就可以到处发布传播了。

**2.Unity 发布游戏**<br>

Unity 的最大的一个特点就是“跨平台运行”，一处开发多处运行。<br>
常用的发布平台：Windows，Android，IOS，Mac，Web......<br>
我们现在演示打包发布Windows 版本的游戏。

## 2.Unity 发布PC 版游戏

**1.Build Settings[生成设置]**<br>

File-->Build Settings 弹出项目生成设置面板。<br>
选择要发布到的平台；<br>
添加要发布的场景；

**2.Player Settings[详细设置]**

Company Name：公司名称<br>
Product Name：产品名称（游戏名称）<br>
Default Icon：默认图标

**3.成品文件介绍**<br>

一个exe 可执行文件，一个Data 数据文件夹，两个缺一不可以，且不可分割。
