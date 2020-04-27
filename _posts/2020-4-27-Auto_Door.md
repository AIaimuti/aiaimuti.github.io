---
layout:     post
title:      UE4 C++入门
subtitle:   基础内容
date:       2020-4-23
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

## 自动门
**功能需求：**踩到地上机关，门开启，且延时关闭
### 触发盒子及机关声明

```
UPROPERTY(EditAnywhere, BlueprintReadWrite)
class UBoxComponent * TriggerBox;

UPROPERTY(EditAnywhere, BlueprintReadWrite)
class UStaticMeshComponent * FloorSwitch;

UPROPERTY(EditAnywhere, BlueprintReadWrite)
class UStaticMeshComponent * Door;

```
### 触发盒子及机关配置
添加头文件：<br>
#include "Components/BoxComponent.h"<br>
#include "Components/StaticMeshComponent.h"<br>
SetCollisionEnabled(ECollisionEnabled::QueryOnly)<br>
ECollisionEnabled 有4个参数<br>
NoCollision 无碰撞<br>
QueryOnly 仅询问，没有物理碰撞，会有重叠<br>
PhysicsOnly 只有物理碰撞，阻挡效果<br>
QueryAndPhysics 两种都关心，询问和物理碰撞

```
TriggerBox = CreateDefaultSubobject<UBoxComponent>(TEXT("TriggerBox"));
RootComponent = TriggerBox;
//选择碰撞类型，仅询问，即可以重叠
TriggerBox->SetCollisionEnabled(ECollisionEnabled::QueryOnly);
//设置自己是世界静态物体
TriggerBox->SetCollisionObjectType(ECollisionChannel::ECC_WorldStatic);
//将所以通道设置为忽略
TriggerBox->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
//对Pawn设置重叠
TriggerBox->SetCollisionResponseToChannel(ECollisionChannel::ECC_Pawn, ECollisionResponse::ECR_Overlap);
//设置盒子大小
TriggerBox->SetBoxExtent(FVector(64, 64, 32));

//门与机关配置
FloorSwitch = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("FloorSwitch"));
FloorSwitch->SetupAttachment(RootComponent);

Door = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Door"));
Door->SetupAttachment(RootComponent);
```
### 相关函数声明
#### BlueprintImplementableEvent
函数在蓝图中实现
```
//上升门
UFUNCTION(BlueprintImplementableEvent)
	void RaiseDoor();
//下降门
UFUNCTION(BlueprintImplementableEvent)
	void LowerDoor();
//上升机关
UFUNCTION(BlueprintImplementableEvent)
	void RaiseFloorSwitch();
//下降机关
UFUNCTION(BlueprintImplementableEvent)
	void LowerFloorSwitch();
```
#### DECLARE_DYNAMIC_MULTICAST_SPARSE_DELEGATE_SixParams
带DECLARE_DYNAMIC_MULTICAST_DELEGATE前缀的是一种动态委托的宏，这种宏采用AddDynamic绑定自定义函数，
自定义函数参数必须和宏定义中的参数声明形式相同，且.h文件中声明自定义函数前面需要加UFUNCTION()
```
//自定义进入碰撞盒子事件
UFUNCTION()
	void OnOverlapBegin(UPrimitiveComponent* OverlappedComponent, 
		AActor* OtherActor, UPrimitiveComponent* OtherComp, 
		int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult);
//自定义进入离开盒子事件
UFUNCTION()
	void OnOverlapEnd(UPrimitiveComponent* OverlappedComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex);
//更新门的位置
UFUNCTION(BlueprintCallable)
	void UpdateDoorLocation(float Z);
//更新机关的位置
UFUNCTION(BlueprintCallable)
	void UpdateSwitchLocation(float Z);
```
### 相关函数定义
添加头文件：
#include "TimerManager.h"
碰撞函数的绑定是动态生成的，不要放在构造函数，可能会出问题，因此放在BeginPlay()
```
void AFloorSwitch::BeginPlay()
{
	Super::BeginPlay();
	TriggerBox->OnComponentBeginOverlap.AddDynamic(this, &AFloorSwitch::OnOverlapBegin);
	TriggerBox->OnComponentEndOverlap.AddDynamic(this, &AFloorSwitch::OnOverlapEnd);
	
}
```
在重合触发盒子的时候，开门，降下机关，同时关闭计时器，
离开触发盒子的时候，关门，开始计时器，起到延时关闭的效果
```
void AFloorSwitch::OnOverlapBegin(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	UE_LOG(LogTemp, Warning, TEXT("Overlap Begin"));
	//关闭计时器
	GetWorldTimerManager().ClearTimer(TimerHandle);
	RaiseDoor();
	LowerFloorSwitch();
}

void AFloorSwitch::OnOverlapEnd(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	UE_LOG(LogTemp, Warning, TEXT("Overlap End"));
	//开启计时器
	GetWorldTimerManager().SetTimer(TimerHandle, this, &AFloorSwitch::CloseDoor, SwitchTime);
	//LowerDoor();
	//RaiseFloorSwitch();
}
void AFloorSwitch::CloseDoor()
{
	LowerDoor();
	RaiseFloorSwitch();
}
```
这两个函数用于蓝图实现，起到改变开关和门的位置的效果
```
void AFloorSwitch::UpdateDoorLocation(float Z)
{
	FVector NewLocation = InitialDoorLocation;
	NewLocation.Z += Z;
	Door->SetWorldLocation(NewLocation);
}

void AFloorSwitch::UpdateSwitchLocation(float Z)
{
	FVector NewLocation = InitialSwitchLocation;
	NewLocation.Z += Z;
	FloorSwitch->SetWorldLocation(NewLocation);
}
```
