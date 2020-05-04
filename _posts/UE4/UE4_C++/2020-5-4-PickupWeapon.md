---
layout:     post
title:      拾取武器
subtitle:   基础内容
date:       2020-5-1
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

今天要做的是一个玩家拾取武器并装备到身上的效果

## 武器item
武器从我们之前创建的item类中继承

### 武器item准备
添加构造函数、触发函数和装备函数
```
public:
	AWeapon();

	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		USkeletalMeshComponent *Mesh;

	virtual void OnOverlapBegin_Item(UPrimitiveComponent* OverlappedComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComp,
		int32 OtherBodyIndex, bool bFromSweep,
		const FHitResult & SweepResult) override;

	void Equip(class AMan *Man);
```

### 武器item函数实现
添加头文件<br>
#include "Components/SkeletalMeshComponent.h"<br><br>
#include "Components/StaticMeshComponent.h"<br>
#include "Components/SphereComponent.h"<br>
#include "Engine/SkeletalMeshSocket.h"<br>
#include "Particles/ParticleSystemComponent.h"<br>
#include "Man.h"<br>
实例化一个骨架网格体，并附着在静态网格体上（父类Item中的实例化过），因为我们要实现的物体旋转是对网格体进行旋转

```
AWeapon::AWeapon()
{
  //实例化一个骨架网格体
	Mesh = CreateDefaultSubobject<USkeletalMeshComponent>(TEXT("Mesh"));
	Mesh->SetupAttachment(MeshComp);
}

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
			Equip(Man);
		}
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
		//关闭碰撞触发函数，避免一直调用触发函数功能
		SphereComp->OnComponentBeginOverlap.RemoveDynamic(this, &AItem::OnOverlapBegin_Item);
	}
}
```
