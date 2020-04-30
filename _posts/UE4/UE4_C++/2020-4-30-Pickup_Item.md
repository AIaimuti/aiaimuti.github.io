---
layout:     post
title:      拾取物品
subtitle:   基础内容
date:       2020-4-29
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
今天做一些物品的拾取，并加入一些简单的特效
## 拾取物品
## 拾取物品准备
物品拾取需要触发函数，这里我们也是自定义的碰撞函数
其他则是一些需要的使用的组件，球形碰撞体、静态网格体组件、特效常态组件、特效触发组件、声音组件
```
UPROPERTY(VisibleAnywhere, BlueprintReadWrite, Category = "Item|Collision")
  class USphereComponent * SphereComp;

//进入碰撞体的触发函数
UFUNCTION()
  virtual void OnOverlapBegin_Item(UPrimitiveComponent* OverlappedComponent,
    AActor* OtherActor, UPrimitiveComponent* OtherComp,
    int32 OtherBodyIndex, bool bFromSweep,
    const FHitResult & SweepResult);
//离开碰撞体的触发函数
UFUNCTION()
  virtual void OnOverlapEnd_Item(UPrimitiveComponent* OverlappedComponent,
    AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex);

//静态网格体
UPROPERTY(VisibleAnywhere, BlueprintReadWrite, Category = "Item|Mesh")
  UStaticMeshComponent *MeshComp;
//粒子组件，物体常规状态下的特效
UPROPERTY(VisibleAnywhere, BlueprintReadWrite, Category = "Item|Particle")
  class UParticleSystemComponent *ParticleComp;
//粒子互动效果
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Item|Particle")
  UParticleSystem *ParticleOverlap;
//声音组件
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Item|Sound")
  class USoundCue *SoundOverlap;
```
