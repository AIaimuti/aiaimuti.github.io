---
layout:     post
title:      浮动飞盘
subtitle:   基础内容
date:       2020-4-29
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/Video/FloatPlatform.gif)

今天要实现的功能是一个飞盘的浮动，这个关卡在游戏中很常见，也很有意思。
## 浮动飞盘
功能需求： 制作一些飞盘，飞盘可以移动，可以把玩家送到指定位置。
### 浮动飞盘准备
1).h文件加入配置
```
UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
  UStaticMeshComponent *Mesh;
//起始点
UPROPERTY(EditAnywhere)
  FVector StartPoint;
//结束点
UPROPERTY(EditAnywhere, meta = (MakeEditWidget = "true"))
  FVector EndPoint;
//漂浮速度
UPROPERTY(EditAnywhere,BlueprintReadOnly)
  float InterpSpeed;
//漂浮(切换)时间
UPROPERTY(EditAnywhere, BlueprintReadOnly)
  float InterpTime;
//切换计时器
FTimerHandle InterpTimer;
//决定向上飘或者向下飘
bool blnterping;
//切换漂浮方向的函数
void ToggleInterping();

```
### 函数实现

