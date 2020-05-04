---
layout:     post
title:      切换武器
subtitle:   基础内容
date:       2020-5-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

<iframe src="//player.bilibili.com/player.html?aid=753065899&bvid=BV17k4y1k7zs&cid=186989994&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> <br>

今天要做的是武器拾取的切换，一般游戏是拾取了武器到背包或者装备栏，目前是按E拾取武器销毁，并前面拾取的武器。

## 切换武器
在上次拾取武器的类Weapon上,进行一些改动

### 切换武器准备
Weapon.h 覆写一个触发离开函数，并加入装备武器的音效
```
//离开触发的函数
virtual void OnOverlapEnd_Item(UPrimitiveComponent* OverlappedComponent,
    AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)override;
//武器装备时的声音
UPROPERTY(EditAnywhere, BlueprintReadOnly)
    USoundCue *SoundEquiped;
```
### 切换武器函数实现
Weapon.cpp添加头文件：<br>
//声音控制<br>
#include "Sound/SoundCue.h"<br>
//播放声音用的<br>
#include "Kismet/GameplayStatics.h"<br>
//用于打印的<br>
#include "Engine/Engine.h"

```
void AWeapon::OnOverlapBegin_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	Super::OnOverlapBegin_Item(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex, bFromSweep, SweepResult);
	if (OtherActor)
	{
		//拿到碰撞触发的实例
		AMan * Man = Cast<AMan>(OtherActor);
		//对实例装备武器
		if (Man)
		{
			//当主角没有接触到其它物体时，另其接触对象为自己，
			//当主角同时接触很多物体时，如果没有这个判断，会让触发和离开触发函数中的变量有些混乱
			//加入ActiveOverlapItem这个变量的判断之后，将逻辑简单化，ActiveOverlapItem只会赋值给第一个物体
			//再接触其它物体时会忽略，这样就只能捡起一把碰到的武器
			if (Man->ActiveOverlapItem == nullptr)
			{
				Man->ActiveOverlapItem = this;
			}
			//显示在屏幕上的相关配置
			TArray <FStringFormatArg> FormatArray;
			FormatArray.Add(GetName());
			GEngine->AddOnScreenDebugMessage(-1, 100, FColor::Blue,
				FString::Format(TEXT("Press E to equip {0}"),FormatArray), true, FVector2D(4,4));

			//Equip(Man);
		}
	}
}

void AWeapon::OnOverlapEnd_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	Super::OnOverlapEnd_Item(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex);
	if (OtherActor)
	{
		AMan *Man = Cast<AMan>(OtherActor);
		//这里也有问题，当我们同时接触两个物体时，离开了第一个物体但没离开第二个物体时，
		//ActiveOverlapItem也会为空，这会导致第二个物体捡不到
		if (Man->ActiveOverlapItem == this)
		{
			Man->ActiveOverlapItem = nullptr;
		}
		GEngine->ClearOnScreenDebugMessages();
	}
}

void AWeapon::Equip(AMan * Man)
{
	if (Man)
	{
		//把武器对于所有通道的碰撞设置为忽略
		Mesh->SetCollisionResponseToAllChannels(ECR_Ignore);
		//设置武器模拟物理为false
		Mesh->SetSimulatePhysics(false);
		//把武器附着在主角身上的函数，三个参数
		//第一个是获取Pawn的网格，第二个是附着的方式，第三个是插槽的名字
		//FAttachmentTransformRules有四种方式
		//保持相对位置KeepRelativeTransform; 保持世界位置KeepWorldTransform; 
		//附着不包括缩放SnapToTargetNotIncludingScale; 附着包括缩放SnapToTargetIncludingScale;
		AttachToComponent(Man->GetMesh(), FAttachmentTransformRules::SnapToTargetNotIncludingScale, "WeaponSocket");
		Rotate = false;
		//关闭粒子特效
		ParticleComp->SetActive(false);
		//解绑定碰撞触发函数，避免一直调用触发函数功能
		SphereComp->OnComponentBeginOverlap.RemoveDynamic(this, &AItem::OnOverlapBegin_Item);
		//解绑定碰撞触发离开函数，避免一直调用触发函数功能
		SphereComp->OnComponentEndOverlap.RemoveDynamic(this, &AItem::OnOverlapEnd_Item);
		GEngine->ClearOnScreenDebugMessages();
		//播放装备声音
		if (SoundEquiped)
		{
			UGameplayStatics::PlaySound2D(this, SoundEquiped);
		}
		//装备武器
		Man->SetWeapon(this);
	}
}
```
