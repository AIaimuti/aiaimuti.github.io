---
layout:     post
title:      生成Actor
subtitle:   基础内容
date:       2020-4-27
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

##  生成Actor
### 生成Actor配置
```
//创建一个碰撞盒子
UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
  class UBoxComponent * SpawningBox;
//返回碰撞盒子内一个随机的点
UFUNCTION(BlueprintPure)
  FVector GetSpawnPoint();

UFUNCTION(BlueprintCallable)
  FVector GetSpawnPointDummy(FVector Value);
//创建一个ACreature的子类PawnToSpawn
UPROPERTY(EditAnywhere, BlueprintReadOnly)
  TSubclassOf<class ACreature> PawnToSpawn;

//BlueprintNativeEvent需要我们做一个原生的实现，但是这个实现可以随时被覆盖
//当在蓝图中使用时，会搜索出一个蓝图和一个事件，使用事件会覆盖函数功能，直接使用函数时有效的
UFUNCTION(BlueprintNativeEvent, BlueprintPure)
  void SpawnMyPawn(UClass *PawnClass, FVector const&Location);
```
