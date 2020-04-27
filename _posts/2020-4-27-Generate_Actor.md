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
**功能需求：**在指定区域内生成若干个Actor。
BlueprintNativeEvent 需要我们做一个函数原生的实现，但是这个实现可以随时被覆盖<br>
当在蓝图中使用时，会搜索出一个蓝图和一个事件，使用事件会覆盖函数功能，直接使用函数时有效的<br>
TSubclassOf<class ACreature> PawnToSpawn 创建一个ACreature的子类PawnToSpawn<br>
BlueprintCallable 一般的可调用函数<br>
BlueprintPrue 纯粹的蓝图函数，没有执行的输入和输出点<br>
生成的Actor不能设置玩家为Player0，不然会出问题
    
```
//创建一个碰撞盒子
UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
  class UBoxComponent * SpawningBox;
//返回碰撞盒子内一个随机的点
UFUNCTION(BlueprintPure)
  FVector GetSpawnPoint();
//创建一个ACreature的子类PawnToSpawn
UPROPERTY(EditAnywhere, BlueprintReadOnly)
  TSubclassOf<class ACreature> PawnToSpawn;

//BlueprintNativeEvent需要我们做一个原生的实现，但是这个实现可以随时被覆盖
//当在蓝图中使用时，会搜索出一个蓝图和一个事件，使用事件会覆盖函数功能，直接使用函数时有效的
UFUNCTION(BlueprintNativeEvent, BlueprintPure)
  void SpawnMyPawn(UClass *PawnClass, FVector const&Location);
```
### 生成Actor函数实现
添加头文件：<br>
#include "Components/BoxComponent.h"<br>
#include "Kismet/KismetMathLibrary.h"<br>
#include "Engine/World.h"<br>
#include "Creature.h"<br>
在蓝图中使用GetSpawnPoint和前面声明好的PawnToSpawn连接到spawnActor就可以完成功能<br>
同样直接调用SpawnMyPawn函数也可以生成Actor<br>
这里介绍了生成Actor的方式
```
ASpawnVolume::ASpawnVolume()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
	SpawningBox = CreateDefaultSubobject<UBoxComponent>(TEXT("SpawningBox"));


}

FVector ASpawnVolume::GetSpawnPoint()
{
	//获得盒子的长宽高
	FVector Extent = SpawningBox->GetScaledBoxExtent();
	//获得盒子中心点的位置
	FVector Origin = SpawningBox->GetComponentLocation();
	//在盒子范围内生成随机的点
	FVector Point = UKismetMathLibrary::RandomPointInBoundingBox(Origin, Extent);
	return Point;
}

//这个可覆盖函数实现时要在函数名后面加_Implementation
void ASpawnVolume::SpawnMyPawn_Implementation(UClass * PawnClass, FVector const & Location)
{
	if (PawnClass)
	{
		UWorld *MyWorld = GetWorld();

		if (MyWorld)
		{
			ACreature *Creature = MyWorld->SpawnActor<ACreature>(PawnClass, Location, FRotator(0.f));
		}
	}	
}
```
