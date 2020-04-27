---
layout:     post
title:      Character
subtitle:   基础内容
date:       2020-4-25
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

## Character摄像机
### 摄像机和弹簧臂配置
1).h文件
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
  class USpringArmComponent * SpringArmComp;

UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
  class UCameraComponent * Camera;
```
