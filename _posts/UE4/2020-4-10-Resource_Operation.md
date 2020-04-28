---
layout:     post
title:      UE4 资源操作
subtitle:   基础内容
date:       2020-4-10
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 入门系列
---

# 导入内容

资源类型|文件扩展名|应用程序
--|:--:|--:
三维模型、骨架网格体结<br>构、动画数据|.fbx、.obj|Maya、3ds Max、<br>ZBrush
纹理和图片|.bmp、.jpeg、.pcx、<br>.png、.psd、.tga、<br>.hdr|Photoshop
字体|.otf、.ttf|BitFontMaker2
音频|.wav|Audacity、Audition
视频和多媒体|.wmv|After Effects、<br>Media Encoder
physh|.apb、.apx|APEX PhysX Lab
其他|.csv|Excel

## 其他资源类型
蓝图类
粒子系统
材质和材质实例

## 导入资源之文件组织
合理的组织文件非常重要。将内容导入引擎之前，应对外部文件进行组织。
要更改原始资源时，可以方便地更新源并使用Reimport命令重新导入。

##命名规范
合理的组织文件还需要遵守命名规范。
Epic编写了风格指南
http://ue4.style/

## 导入资源之资源图标  
资源图标右下角的小星号表示资源尚未保存
导入的资源必须保存才能写进磁盘
如果关闭编辑器，未保存的导入资源将消失
按Ctrl + S保存所有资源，或右单击资源并选择保存

## 引用查看器
快捷键 Alt + Shift + R
大多数资源依赖于一个或者多个其它资源

例如，如果在静态网格体编辑器中将一个材质分配给静态网格资源，那么静态网格体现在会引用该资源。

这意味着当加载静态网格资源时，还需要加载引用的材质。

相应地，材质编辑器也将加载材质编辑器中引用的纹理。

## 使用网上下载的素材包
一定要使用高版本的打开低版本的资源
1)直接复制粘贴时，打开文件-->data-->Content-->目标复制文件夹
然后复制到我们目录下的Content文件夹内
2）将一个工程的文件夹/蓝图迁移到另一个工程
右键选择迁移(migrate)来迁移到另一个文件夹的Content目录内
这样迁移会不仅会将需要迁移的文件夹进行迁移，还会将所需迁移的文件夹内文件用到的所有材质、贴图、动画一起迁移过去。

# 免费素材
## Epic Game商城
免费项目、本月免费、内容示例里面所有素材免费
youtube unreal free download
ue4 free free project download
官方论坛、帖子
