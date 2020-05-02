---
layout:     post
title:      浮动飞盘
subtitle:   基础内容
date:       2020-4-29
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/Gif/FloatPlatform_out.gif)

今天要实现的功能是一个飞盘的浮动，这个关卡在游戏中很常见，也很有意思。
## 浮动飞盘
功能需求： 制作一些飞盘，飞盘可以移动，可以把玩家送到指定位置。
### 浮动飞盘准备
1).h文件加入配置
加入了用于放置飞盘静态网格体，飞盘起始点和结束点，计时器函数，以及切换方向的函数
```
UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
  UStaticMeshComponent *Mesh;
//起始点
UPROPERTY(EditAnywhere)
  FVector StartPoint;
//结束点
UPROPERTY(EditAnywhere, meta = (MakeEditWidget = "true"))
  FVector EndPoint;
//漂浮速度
UPROPERTY(EditAnywhere,BlueprintReadOnly)
  float InterpSpeed;
//漂浮(切换)时间
UPROPERTY(EditAnywhere, BlueprintReadOnly)
  float InterpTime;
//切换计时器
FTimerHandle InterpTimer;
//决定向上飘或者向下飘
bool blnterping;
//切换漂浮方向的函数
void ToggleInterping();

```
### 函数实现
初始化网格体和切换速度和时间
```
AFloatPlatform::AFloatPlatform()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

	Mesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
	RootComponent = Mesh;

	InterpSpeed = 1;
	InterpTime = 5;
}
```
设置状态转换函数
获取飞盘本身的绝对位置，相对于物体本身的绝对位置设置飞盘的初始位置和结束位置，并设置循环计数器。
```
void AFloatPlatform::ToggleInterping()
{
	//状态转换
	blnterping = !blnterping;
}

// Called when the game starts or when spawned
void AFloatPlatform::BeginPlay()
{
	Super::BeginPlay();

	//获取飞盘初始位置
	//用户编辑时是相对于物体本身的绝对位置，有利于位置调整
	StartPoint = GetActorLocation();
	//先让初始位置等于结束位置
	EndPoint += StartPoint;

	blnterping = true;
	//获取并设置计时器，InterpTimer是计时器，ToggleInterping是调用的函数，InterpTime是计时时间，
	//true开启是循环，每过InterpTime时间，调用ToggleInterping函数
  
	GetWorldTimerManager().SetTimer(InterpTimer, this, &AFloatPlatform::ToggleInterping, InterpTime, true);
	
}
```
每帧中调用的函数，根据blnterping的状态不同，计算每帧移动的距离，对飞盘进行移动
```
void AFloatPlatform::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	//获取飞盘初始位置
	FVector CurrentLocation = GetActorLocation();
	FVector TargetLocation;

	if (blnterping)
	{
		//计算速度为InterpSpeed情况下，每帧向结束点运动的距离
		TargetLocation = FMath::VInterpTo(CurrentLocation, EndPoint, DeltaTime, InterpSpeed);
	}
	else
	{	//计算速度为InterpSpeed情况下，每帧向起始点运动的距离
		TargetLocation = FMath::VInterpTo(CurrentLocation, StartPoint, DeltaTime, InterpSpeed);	
	}
	SetActorLocation(TargetLocation);
}
```

