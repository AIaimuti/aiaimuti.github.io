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

<iframe src="//player.bilibili.com/player.html?aid=840553586&bvid=BV1T54y1X7df&cid=186152489&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

今天做一些物品的拾取，并加入一些简单的特效<br>
## 拾取物品
## 拾取物品准备
物品拾取需要触发函数，这里我们也是自定义的碰撞函数<br>
自定义的碰撞函数也是像之前一样，需要设置好参数并动态绑定<br>
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
## 函数实现

首先添加相应头文件<br>
#include "Components/SphereComponent.h"<br>
#include "Components/StaticMeshComponent.h"<br>
//粒子组件<br>
#include "Particles/ParticleSystemComponent.h"<br>
//用来生成粒子的<br>
#include "Kismet/GameplayStatics.h"<br>
#include "Sound/SoundCue.h"

另外，这里会加一个测试用的函数<br>
`UE_LOG(LogTemp, Warning, TEXT("%s"), *FString(__FUNCTION__));`<br>
我们知道UE_LOG是一个在能打印在输出窗口的函数，* FString(__FUNCTION__)则是可以打印出所调用函数的类和父类，这样我们就能清楚知道是哪个类在调用函数<br>

组件的的相关配置
```
AItem::AItem()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
    //添加球形组件
	SphereComp = CreateDefaultSubobject<USphereComponent>(TEXT("SphereComp"));
	RootComponent = SphereComp;
    //添加静态网格体
	MeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("MeshComp"));
	MeshComp->SetupAttachment(RootComponent);
    //添加特效组件
	ParticleComp = CreateDefaultSubobject<UParticleSystemComponent>(TEXT("ParticleComp"));
	ParticleComp->SetupAttachment(RootComponent);

}
```
这里是粒子触发特效和声音特效在一起的用法，在进入触发盒子的时候调用
```
void AItem::OnOverlapBegin_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	//用于显示出当前函数所属的类
	UE_LOG(LogTemp, Warning, TEXT("%s"), *FString(__FUNCTION__));

	if (ParticleOverlap)
	{
		//生成一个粒子发射器，在Actor所在的位置，生成ParticleOverlap效果
		UGameplayStatics::SpawnEmitterAtLocation(GetWorld(), ParticleOverlap, GetActorLocation());

	}
	if (SoundOverlap)
	{
		//播放声音
		UGameplayStatics::PlaySound2D(GetWorld(), SoundOverlap);
	}
	Destroy();
}

void AItem::OnOverlapEnd_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	UE_LOG(LogTemp, Warning, TEXT("%s"), *FString(__FUNCTION__));
}

// Called when the game starts or when spawned
void AItem::BeginPlay()
{
	Super::BeginPlay();
	SphereComp->OnComponentBeginOverlap.AddDynamic(this, &AItem::OnOverlapBegin_Item);
	SphereComp->OnComponentEndOverlap.AddDynamic(this, &AItem::OnOverlapEnd_Item);
}
```
由此，可以衍生一些子类去用不同的特效，比如炸弹爆炸、金币拾取。
