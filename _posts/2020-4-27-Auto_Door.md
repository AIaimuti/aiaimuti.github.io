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
SetCollisionEnabled(ECollisionEnabled::QueryOnly)
ECollisionEnabled 有4个参数
NoCollision 无碰撞
QueryOnly 仅询问，没有物理碰撞，会有重叠
PhysicsOnly 只有物理碰撞，阻挡效果
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
UFUNCTION(BlueprintImplementableEvent)
	void RaiseDoor();
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
//更新门的位置
UFUNCTION(BlueprintCallable)
	void UpdateDoorLocation(float Z);
//更新机关的位置
UFUNCTION(BlueprintCallable)
	void UpdateSwitchLocation(float Z);
```
碰撞函数的绑定是动态生成的，不要放在构造函数，可能会出问题
```
void AFloorSwitch::BeginPlay()
{
	Super::BeginPlay();
	TriggerBox->OnComponentBeginOverlap.AddDynamic(this, &AFloorSwitch::OnOverlapBegin);
	TriggerBox->OnComponentEndOverlap.AddDynamic(this, &AFloorSwitch::OnOverlapEnd);
	
}
```
#### 相关函数定义
1）初始化绑定
