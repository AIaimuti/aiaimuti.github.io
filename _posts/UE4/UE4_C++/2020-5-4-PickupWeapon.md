---
layout:     post
title:      拾取武器
subtitle:   基础内容
date:       2020-5-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
<iframe src="//player.bilibili.com/player.html?aid=883120949&bvid=BV1LK4y187mh&cid=186843564&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
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
#include "Components/SkeletalMeshComponent.h"<br>
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
```
触发时调用Equip函数；<br>
将武器碰撞、模拟物理关闭；<br>
将武器附着在人物骨骼网格体上的WeaponSocket插槽上，这个插槽我们放在右手上；<br>
然后关闭物体的旋转，粒子特效以及触发函数，避免出现一直调用触发函数功能的情况；
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
### Item父类中旋转设置
其中Rotate参数在Item父类中进行的初始化
Item.h中设置是否旋转、旋转速度和旋转函数
```
	//是否旋转
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Item|Rotate")
	bool Rotate;
	//旋转速度
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Item|Rotate")
	float RotationSpeed;
	//旋转函数
UFUNCTION()
	void Rotation(float DeltaTime);
```
Item.cpp中
首先初始化旋转参数
```
AItem::AItem()
{
///////////////这里省略了其它设置///////////////////
	//初始化旋转参数
	Rotate = true;
	RotationSpeed = 30;
}
```
然后实现旋转函数，并在Tick函数中调用
```
void AItem::Rotation(float DeltaTime)
{
	if (Rotate)
	{	//获取物体旋转
		FRotator Rotation = MeshComp->GetComponentRotation();
		//计算每帧增加的旋转角度
		Rotation.Yaw += DeltaTime * RotationSpeed;
		//对静态网格体进行旋转设置
		MeshComp->SetWorldRotation(Rotation);
	}
}

// Called every frame
void AItem::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	Rotation(DeltaTime);

}
```
