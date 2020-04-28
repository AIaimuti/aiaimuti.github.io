---
layout:     post
title:      控制移动+完善运动+镜头摇臂+自制移动组件+镜头调整
subtitle:   基础内容
date:       2020-4-24
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

## 控制移动
**功能需求:** 能够通过WASD进行简单移动<br>
### 移动准备
1)先Project Setting添加轴映射-->添加Components/InputComponent.h头文件-->SetupPlayerInputComponent中绑定函数<br>
2).h文件中声明
```
void MoveForward(float Value);

void MoveRight(float Value);
```
3).cpp文件中定义<br>
在构造函数中，让0号玩家拥有控制权
`AutoPossessPlayer = EAutoReceiveInput::Player0;`

### 移动函数
```
void ACreature::MoveForward(float Value)
{
	CurrentVelocity.X = FMath::Clamp(Value, -1.f, 1.f)* MaxSpeed;
}

void ACreature::MoveRight(float Value)
{
	CurrentVelocity.Y = FMath::Clamp(Value, -1.f, 1.f)* MaxSpeed;
}
```
将速度限制到-1到1 * MaxSpeed
`CurrentVelocity.X = FMath::Clamp(Value, -1.f, 1.f)* MaxSpeed;`

FMath::Clamp(Value,min,max)
限制Value介于max和min之间

在Tick函数中
```
void ACreature::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	//新位置等于当前位置+速度*每帧时间
	FVector NewLocation = GetActorLocation() + CurrentVelocity * DeltaTime;
	SetActorLocation(NewLocation);

}
```


## 完善运动
**功能需求:** 简单的写法没有考虑脸的朝向，这种写法考虑了脸的朝向，角色转身后也是脸的正向<br>
AddInputVector:将给定向量添加到世界空间的累积输入中。输入向量的大小通常在0到1之间。它们在帧中累积，然后在运动更新期间用作加速度。
```
virtual void AddInputVector
(
    FVector WorldVector,
    bool bForce
)
```
WorldVector:在世界空间中应用输入的方向<br>
bForce:如果为true，则始终添加输入

GetActorForwardVector:获取Actor在世界空间的正向(X)方向，取值-1，0，1<br>
GetActorRightVector：获取Actor在世界空间的右向(Y)方向，取值-1，0，1
```
void ACreature::MoveForward(float Value)
{
	if (MovementComp)
	{
		MovementComp->AddInputVector(GetActorForwardVector() * Value);
	}

}

void ACreature::MoveRight(float Value)
{
	if (MovementComp)
	{
		MovementComp->AddInputVector(GetActorRightVector() * Value);
	}
}
```

## 摄像机弹簧臂
**功能需求:** 摄像机动态追踪物体，并且有一定延迟效果。<br>

### 设置球形碰撞体配置
1).h文件声明
```
UPROPERTY(VisibleAnywhere)
	class USphereComponent * SphereCollision;
```
2).cpp配置<br>
添加头文件：<br>
#include "Components/SphereComponent.h"<br>
#include "Components/StaticMeshComponent.h"<br>
#include "Uobject/ConstructorHelpers.h"
```
//实例化球形组件
SphereCollision = CreateDefaultSubobject<USphereComponent>(TEXT("SphereCollision"));
//设置半径
SphereCollision->SetSphereRadius(80);
//设置初始碰撞类型
SphereCollision->SetCollisionProfileName(TEXT("Block All"));
//设置球形组件是否隐藏，不设置的话默认隐藏
SphereCollision->SetHiddenInGame(false);
//设置根组件
SetRootComponent(SphereCollision);
```
#### ConstructorHelpers直接指定并添加静态网格体资源
直接指定网格资源采用如下方式，需要添加Uobject/ConstructorHelpers.h头文件。<br>
TEXT内部引用的路径：打开actor蓝图-->Static Mesh点放大镜打开内容浏览器-->在内容浏览器中右键点击所选物体-->点击Copy Reference
```
//使用ConstructorHelpers找指定位置的静态网格体资源给MeshComponentAsset
static ConstructorHelpers::FObjectFinder<UStaticMesh>MeshComponentAsset
        (TEXT("StaticMesh'/Game/StarterContent/Shapes/Shape_Sphere.Shape_Sphere'"));

if (MeshComponentAsset.Succeeded())
	{
        //如果寻找成功，放到静态网格体中，并设置位置
		MeshComponent->SetStaticMesh(MeshComponentAsset.Object);
		MeshComponent->SetRelativeLocation(FVector(0.f, 0, -50));
	}
```
有时编译好了会没有变化，这时点类设置，换一下蓝图的父类，然后再还回去

### 摄像机弹簧臂配置
设置了弹簧臂长度、位置以及作为谁的附件，并开启摄像机延迟<br>
#### SetupAttachment函数
```
void SetupAttachment
(
    USceneComponent * InParent,
    FName InSocketName
)
```
InParent：指定依附对象<br>
InSocketName：指定插槽名称

弹簧臂一段连接物体原点，另一端自带插槽。这里将摄像机绑定弹簧臂的插槽，不然摄像机会绑定到弹簧臂的原点
`Camera->SetupAttachment(SpringArmComp, USpringArmComponent::SocketName);`
#### 配置
1).h文件声明
```
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Camera")
	class UCameraComponent * Camera;

UPROPERTY(VisibleAnywhere)
	class USpringArmComponent * SpringArmComp;
```
2).cpp文件配置<br>
添加头文件：<br>
#include "Camera/CameraComponent.h"<br>
#include "GameFramework/SpringArmComponent.h"
```
//实例化弹簧臂组件
SpringArmComp = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArmComp"));
//弹簧臂组件组件附着在根组件
SpringArmComp->SetupAttachment(RootComponent);
//弹簧臂组件长度设为400
SpringArmComp->TargetArmLength = 400;
//设置弹簧臂组件位置
SpringArmComp->SetRelativeRotation(FRotator(-45.f, 0, 0));
//开启摄像机延迟
SpringArmComp->bEnableCameraLag = true;
//设置摄像机延迟速度
SpringArmComp->CameraLagSpeed = 4;

Camera = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));
//摄像机绑定弹簧臂，位置是插槽的位置，如果没有指定，摄像机会绑定在弹簧臂自身原点的位置
//弹簧臂的另一端自带插槽
Camera->SetupAttachment(SpringArmComp, USpringArmComponent::SocketName);
```

## 自制动作组件
**功能需求:** 球体碰撞到墙壁时，沿边移动。<br>

### 自制动作组件准备
1)创建一个继承自UPawnMovementComponent的C++类<br>
`class UGDC_API UMyPawnMovementComponent : public UPawnMovementComponent`<br>

2)覆写 TickComponent以形成我们的自定义运动组件<br>
从父类依次寻找找到函数定义<br>
UPawnMovementComponent-->UNavMovementComponent-->UMovementComponent<br>
UMovementComponent中有函数TickComponent<br>
`virtual void TickComponent(float DeltaTime, enum ELevelTick TickType, FActorComponentTickFunction *ThisTickFunction) override;`

3)Creature中声明自制运动组件<br>
Creature.h文件中声明<br>
首先定义UMyPawnMovementComponent类的MovementComp变量；<br>
覆写UPawnMovementComponent的GetMovementComponent()函数，返回我们指定的运动组件来控制Pawn的运动
```
UPROPERTY(VisibleAnywhere)
    class UMyPawnMovementComponent * MovementComp;

virtual UPawnMovementComponent * GetMovementComponent() const override;
```
Creature.cpp文件中<br>
实例化继承后的UMyPawnMovementComponent类MovementComp，并指定根组件作为我们移动和更新的标准<br>
GetMovementComponent()中使用我们自定义的移动组件<br>
并添加头文件：<br>
#include "..\Public\MyPawnMovementComponent.h"
```
MovementComp = CreateDefaultSubobject<UMyPawnMovementComponent>(TEXT("MovementComp"));
//设置移动和更新的标准，以根组件我们移动和更新的标准
MovementComp->UpdatedComponent = RootComponent;
    
UPawnMovementComponent * ACreature::GetMovementComponent() const
{   //使用我们自定义的移动组件
	return MovementComp;
}
```
### 自制动作组件内容
条件判断-->获取运动向量-->sweep扫描并实现溜边运动

#### 条件判断
PawnOwner：运动是必须要有Owner的，没有会返回空向量<br>
UpdatedComponent：UpdatedComponent指定以某个组件的运动和更新视为整体的运动和更新，在Creature.cpp 87行设置了以根组件为准<br>
ShouldSkipUpdate(DeltaTime):允许自动跳过更新，有时候没有对Pawn进行操作，就不需要对其更新
```
//运动是必须要有Owner的，没有会返回空向量
//UpdatedComponent指定以某个组件的运动和更新视为整体的运动和更新，在Creature.cpp 87行设置了以根组件为准
//允许自动跳过更新，有时候没有对Pawn进行操作，就不需要对其更新

if (!PawnOwner || !UpdatedComponent || ShouldSkipUpdate(DeltaTime))
{
    return;
}
```
#### 获取运动向量
ConsumeInputVector():消费者，消费运动向量<br>
GetClampedToMaxSize(1): 约束最大长度为1
```
//ConsumeInputVector消费运动向量
//GetClampedToMaxSize约束最大长度为1
//功能是获取运动向量的方向，然后按这个方向以150的速度运动
FVector DeltaMovement
    = ConsumeInputVector().GetClampedToMaxSize(1) * DeltaTime * 150;
```

#### sweep扫描并实现溜边运动
IsNearlyZero:在指定小数点阈值内输出true,用于容错<br>
SafeMoveUpdatedComponent：进行运动的试算，并开启sweep扫描，有阻挡会输出撞击信息<br>
HitResult.IsValidBlockingHit：检测是有有效的撞击<br>
SlideAlongSurface：延边滑动
```
virtual FVector ComputeSlideVector
(
    const FVector & Delta,
    const float Time,
    const FVector & Normal,
    const FHitResult & Hit
) const
```
Delta:尝试移动的方向<br>
Time：通常为1-碰撞时间，即1-Hit.Time<br>
Normal：法向，正常的相对运动方向<br>
Hit：碰撞结果

```
//如果有运动向量输入，IsNearlyZero在指定小数点阈值内输出true，非0就是有运动向量输入
if (!DeltaMovement.IsNearlyZero())
{
    FHitResult HitResult;
    //采用SafeMoveUpdatedComponent进行运动的试算，其中sweep参数设为true
    SafeMoveUpdatedComponent(DeltaMovement,
        UpdatedComponent->GetComponentRotation(), true, HitResult);
    //如果有有效的阻挡碰撞，就溜边运动，HitResult.Normal法向
    if (HitResult.IsValidBlockingHit())
    {
        SlideAlongSurface(DeltaMovement,
            1 - HitResult.Time, HitResult.Normal, HitResult);
    }
}
```

## 调整镜头
**功能需求:** 可以对视角进行左右和上下调整<br>

### 调整镜头准备
1)先Project Setting添加轴映射，CameraPitch-->鼠标Y(Y一般是-1)，CameraYaw-->鼠标X(X一般是1)<br>
2).h文件中设置
```
//视角调整函数
void AngleAdjustment();
//获取鼠标Y输入	
void CameraPitch(float Value);
//获取鼠标X输入
void CameraYaw(float Value);
//记录鼠标变化量
FVector2D CameraInput;
```
3).cpp文件中配置
```
void ACreature::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	AngleAdjustment();
}
```
```
void ACreature::AngleAdjustment()
{
	//获取当前Actor的旋转状态
	FRotator NewRotation = GetActorRotation();
	//将鼠标yaw输入加到NewRotation.Yaw中
	NewRotation.Yaw += CameraInput.X;
	//更新Actor的旋转状态
	SetActorRotation(NewRotation);

	//获取当前弹簧臂的旋转状态
	FRotator NewSpringArmRotation = SpringArmComp->GetComponentRotation();
	//将鼠标pitch输入加到NewRotation.Pitch中
	NewSpringArmRotation.Pitch += CameraInput.Y;
	//将NewRotation.Pitch限制到(-80.f, -15.f)之间
	NewSpringArmRotation.Pitch = FMath::Clamp(NewSpringArmRotation.Pitch, -80.f, -15.f);
	//更新弹簧臂的旋转状态
	SpringArmComp->SetWorldRotation(NewSpringArmRotation);
}


void ACreature::CameraPitch(float Value)
{
	//获取鼠标Y输入
	CameraInput.Y = Value;
}

void ACreature::CameraYaw(float Value)
{
	//获取鼠标X输入
	CameraInput.X = Value;
}
```
