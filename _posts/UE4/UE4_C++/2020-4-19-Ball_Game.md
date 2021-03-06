---
layout:     post
title:      UE4 C++系列之滚球小游戏
subtitle:   基础内容
date:       2020-4-19
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++系列之滚球小游戏
---

<iframe src="//player.bilibili.com/player.html?aid=583000559&bvid=BV1a64y1M7jr&cid=186331733&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## UE4C++实际用途
1）游戏算法：当某个游戏算法十分复杂的时候，比如弹道计算，采用蓝图开发效率比较低<br>
2）C++网络通信：当多人联机时需要C++写一些通信方面的内容。<br>
3）游戏引擎深度定制：当游戏项目很大时，游戏开发引擎不能满足所有的功能，需要采用C++深度开发

## 常见的宏-UPROPERTY
UPROPERTY 用途广泛。它允许变量、组件被复制、被序列化，并可从蓝图中进行访问。垃圾回收器还使用它们来追踪对 UObject 的引用数。<br>
### EditAnywhere / VisibleAnywhere
EditAntwhere 表示此属性可以通过属性窗口，原型和实例进行编辑（原型指的是类模板，实例指的是具体的对象实例）<br>
VisibleAnywhere 指示此属性在所有属性窗口中都可见，但无法编辑。这个标签与“Edit”标签不兼容
```
UPROPERTY(VisibleAnywhere, Category = "Snowing|Visible")
    FString VisibleAnywhereParam;

UPROPERTY(EditAnywhere, Category = "Snowing|Edit")
    float EditAnywhereParam;
```
![](https://img-blog.csdn.net/20171109100457996?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjc5MzEwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### BlueprintReadWrite
设置属性为蓝图读写。会在蓝图脚本中为被修饰的变量提供 Get 和 Set 方法
```
UPROPERTY(BlueprintReadWrite, Category = "Snowing|Blueprint")
    float BlueprintReadWriteParam;
```
![](https://img-blog.csdn.net/20171108170010210?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjc5MzEwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### BluprintReadOnly
设置属性为蓝图只读。会在蓝图脚本中为被修饰的变量提供 Get 方法，没有 Set 方法
```
UPROPERTY(BlueprintReadOnly, Category = "Snowing|Blueprint")
    float BlueprintReadOnlyParam;
```
![](https://img-blog.csdn.net/20171108165721356?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjc5MzEwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Category 
指定在Blueprint编辑工具中显示的属性的类别。使用|定义嵌套层级
```
UPROPERTY(VisibleAnywhere, Category = "Snowing|Visible")
    FString VisibleAnywhereParam;

UPROPERTY(VisibleInstanceOnly, Category = "Snowing|Visible")
    FString VisibleInstanceOnlyParam;

UPROPERTY(EditAnywhere, Category = "Snowing|Edit")
    float EditAnywhereParam;

UPROPERTY(EditInstanceOnly, Category = "Snowing|Edit")
    float EditInstanceOnlyParam;
```
![](https://img-blog.csdn.net/20171108205714775?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjc5MzEwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 常见的宏-UFUNCTION
基本功能：定义能够被UE识别的函数
### BlueprintCallable
该函数可以在蓝图或关卡蓝图图表中执行
```
public: 
    UFUNCTION(BlueprintCallable, Category = "Snowing,BlueprintFunc")
        void BlueprintCallableFunction();
```
![](https://img-blog.csdn.net/20171110103239399?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjc5MzEwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 游戏主要功能
## 文件架构

## 添加摄像机功能
### 声明和添加组件
采用UPROPERTY()声明UStaticMeshComponent组件
然后需要在.cpp文件中添加头文件Components/StaticMeshComponent.h
同理，USpringArmComponent和UCameraComponent也需要声明；
.cpp文件中也需要添加#include GameFramework/SpringArmComponent.h和Camera/CameraComponent.h

### SetSimulatePhysics设置刚体
`SphereMeshComp->SetSimulatePhysics(true);//开启模拟物理，设置刚体`

### SetupAttachment()函数，指定作为谁的附件
`CameraArmComp->SetupAttachment(SphereMeshComp);//CameraArmComp作为SphereMeshComp的附件`

### 摄像机lag延迟功能
CameraArm组件-->details面板-->lag-->Enable Camera Lag-->Camera Lag  = 4.0<br>
这样摄像机有平滑效果，会比较舒服

### 功能实现
**功能需求:** 在我们添加物体之后，摄像机追踪物体的运动，相当于蓝图中AddComponent添加组件。<br>
1)首先我们需要在.h文件中添加三个组件，分别是静态网格体，摄像机手臂、摄像机。<br>
这里在初始化的地方添加，因为只在开始的时候声明一次。
```
public:
ASphereBase();
//添加静态网格体、摄像机手臂、摄像机
UPROPERTY(EditAnywhere,BlueprintReadWrite,Category = "SphereMeshComp")
	class UStaticMeshComponent * SphereMeshComp;

UPROPERTY(EditAnywhere,BlueprintReadWrite,Category = "CameraArmComp")
	class USpringArmComponent * CameraArmComp;

UPROPERTY(EditAnywhere,BlueprintReadWrite,Category = "CameraComp")
	class UCameraComponent * CameraComp;
 ```      
2)然后在.cpp文件中实例化，SphereMeshComp使用SetSimulatePhysics设置刚体；SetupAttachment()函数，指定作为谁的附件
```
//实例化静态网格体、摄像机手臂和摄像机，并依次作为附件
SphereMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("SphereMeshComp"));
SphereMeshComp->SetSimulatePhysics(true);

CameraArmComp = CreateAbstractDefaultSubobject<USpringArmComponent>(TEXT("CameraArmComp"));
CameraArmComp->SetupAttachment(SphereMeshComp);

CameraComp = CreateAbstractDefaultSubobject<UCameraComponent>(TEXT("CameraComp"));
CameraComp->SetupAttachment(CameraArmComp);
```
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/Component.png?raw=true)


## 移动和加速功能
### 声明和添加组件
#### SetupPlayerInputComponent函数
继承自Actor及其子类，都会默认声明SetupPlayerInputComponent函数，用于进行一些轴映射以及按键映射的函数绑定。
`virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;`
然后需要在.cpp文件中添加头文件Components/InputComponent.h<br>
另外在函数定义处会默认初始化
`Super::SetupPlayerInputComponent(PlayerInputComponent);`
官方文档中是允许Pawn设置自定义输入绑定

### BindAxis和BindAction函数
BindAxis函数有三个参数：事先设置好的轴映射名称、this(源码中是UserClass* Object)和需要绑定的函数<br>
BindAction函数有四个参数：事先设置好的轴映射名称、按键事件、this(源码中是UserClass* Object)和需要绑定的函数
```
PlayerInputComponent->BindAxis("Moveforward",this,&ASphereBase::Moveforward);
PlayerInputComponent->BindAxis("MoveRight",this,&ASphereBase::MoveRight);
PlayerInputComponent->BindAction("SpeedUp", IE_Pressed, this, &ASphereBase::SpeedUp);
PlayerInputComponent->BindAction("SpeedUp", IE_Released, this, &ASphereBase::SpeedLow);
```
### SetPhysicsAngularVelocity函数
`SetPhysicsAngularVelocity(FVector NewAngVel, bool bAddToCurrent = false, FName BoneName = NAME_None)`
第一个参数往哪个方向添加速度，第二个参数是否速度叠加，参数为true的情况下，速度持续叠加，会变得很快

### 功能实现
**功能需求**：按下wsad前后左右移动，按下空格或者shift速度变快。<br>
1)Projrct Setting-->Engine-->input中设置好轴映射和按键映射<br>
2).h文件中添加函数声明，轴映射一般是持续的数值，所以加一个float参数。
```
//声明移动和加速函数
UFUNCTION(BlueprintCallable)
	void Moveforward(float val);

UFUNCTION(BlueprintCallable)
	void MoveRight(float val);

UFUNCTION(BlueprintCallable)
	void SpeedUp();

UFUNCTION(BlueprintCallable)
	void SpeedLow();
```
3).cpp文件中添加函数和轴映射绑定函数
```
void ASphereBase::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	//将设置好的轴映射和按键映射绑定函数
	PlayerInputComponent->BindAxis("Moveforward",this,&ASphereBase::Moveforward);
	PlayerInputComponent->BindAxis("MoveRight",this,&ASphereBase::MoveRight);
	PlayerInputComponent->BindAction("SpeedUp", IE_Pressed, this, &ASphereBase::SpeedUp);
	PlayerInputComponent->BindAction("SpeedUp", IE_Released, this, &ASphereBase::SpeedLow);
}

//前后移动功能
void ASphereBase::Moveforward(float val)
{
	if (IsInput) 
	{
		AngularVelocity.Y = SphereSpeed * val;
	}
}
//左右移动功能
void ASphereBase::MoveRight(float val)
{
	if (IsInput) 
	{
		AngularVelocity.X = -SphereSpeed * val;
	}
}
//加速功能
void ASphereBase::SpeedUp() 
{
	SphereSpeed = SpeedMax;
}
void ASphereBase::SpeedLow()
{
	SphereSpeed = SpeedMin;
}
```
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/Axis.png?raw=true)

3)实现按键放开了也会按指定方向移动，判断是否开启IsInput 且小球是否离开原点，如果是，小球的速度就是持续的。<br>
然后在.h文件中声明和.cpp文件中定义
```
//.h文件
UFUNCTION(BlueprintCallable)
	void SpeedControl();
//.cpp文件
void ASphereBase::SpeedControl()
{
	if (IsInput && AngularVelocity != FVector(0, 0, 0))
	{
		SphereMeshComp->SetPhysicsAngularVelocity(AngularVelocity);
	}
}
```

## 碰撞基类
### 添加碰撞盒子
采用UPROPERTY()声明BoxComponent组件
然后需要在.cpp文件中添加头文件Components/BoxComponent.h
另外为了HitBoxComp可以附着到其它物体上做关卡，这里还设置一个BaseScene组件(具体为啥我也不知道)
然后需要在.cpp文件中添加头文件Components/SceneComponent.h

### 自定义碰撞函数
```
UFUNCTION()
void BeginHit(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult);
```
这里设置的BeginHit函数是一个自定义的碰撞函数，但是需要与OnComponentBeginOverlap函数定义处的参数相同；
`HitBoxComp->OnComponentBeginOverlap.AddDynamic(this, &AHitBoxBase::BeginHit);`
另外在碰撞盒子绑定函数时，使用.AddDynamic()函数，第一个参数是this(源码中是UserClass* Object)，第二个是需要绑定的函数
这里我理解为，OnComponentBeginOverlap设置自定义的碰撞初始化函数时，使用AddDynamic去绑定某个函数

### 碰撞事件虚函数
因为我们需要根据碰撞结果扩展很多功能，如如位置重置、关卡保存以及结束检测等功能，所以这里写一个虚函数用于后续使用
`virtual void OnHitSphere(class ASphereBase * sphere);`

### 功能实现
1)添加碰撞盒子和Scene组件并实例化，碰撞盒子绑定自定义函数需要用到.AddDynamic()函数
```
//.h文件
public:	
//添加Scene组件
UPROPERTY(EditAnywhere, BlueprintReadWrite)
	class USceneComponent * BaseScene;
//添加碰撞盒子
UPROPERTY(EditAnywhere, BlueprintReadWrite)
	class UBoxComponent * HitBoxComp;
	
//.cpp文件
//实例化Scene组件
BaseScene = CreateAbstractDefaultSubobject<USceneComponent>(TEXT("BaseScene"));

//实例化碰撞盒子
HitBoxComp = CreateAbstractDefaultSubobject<UBoxComponent>(TEXT("HitBoxComp"));
HitBoxComp->OnComponentBeginOverlap.AddDynamic(this, &AHitBoxBase::BeginHit);
HitBoxComp->SetupAttachment(BaseScene);
```
2）自定义碰撞检测函数和碰撞事件函数(碰撞事件函数写成虚函数，因为经常可能用到和自定义功能)<br>
虚函数定义，注意变量前面有class，如virtual void OnHitSphere(class ASphereBase * sphere);
```
//.h文件
//声明自定义OnCollision函数
UFUNCTION()
void BeginHit(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult);
//声明碰撞事件虚函数，
virtual void OnHitSphere(class ASphereBase * sphere);

//.cpp文件
void AHitBoxBase::BeginHit(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	if (Cast<ASphereBase>(OtherActor)) 
	{
		ASphereBase * sphere = Cast<ASphereBase>(OtherActor);
		OnHitSphere(sphere);
	}
}
//定义的虚函数，用于override
void AHitBoxBase::OnHitSphere(ASphereBase * sphere)
{

}
```

## 碰撞子类--位置重置(DieShpere)
### 虚函数覆写
`virtual void OnHitSphere(class ASphereBase * sphere) override;`
这里虚函数是重新移动Pawn的位置
### 功能实现
**功能需求**：小球碰到碰撞盒子重置位置。<br>
1)关于碰撞函数和碰撞事件HitBoxBase文件中，从HitBoxBase类中继承并覆写OnHitSphere函数
```
public:
	//虚函数OnHitSphere的override
	virtual void OnHitSphere(class ASphereBase * sphere) override;
};
```
2).cpp文件中定义覆写OnHitSphere函数，其中，函数定义中涉及ATestGameGameModeBase和GetWorld，<br>
所以要添加TestGameGameModeBase.h和Engine.h到头文件。
```
void ADieSphere::OnHitSphere(ASphereBase * sphere)
{	//
	ATestGameGameModeBase * GameModeBase = Cast<ATestGameGameModeBase>(GetWorld()->GetAuthGameMode());

	GameModeBase->SetPlayerLocation();
}
```

## 碰撞子类--关卡保存(SaveLocation)
### 添加静态网格体
采用UPROPERTY()声明UStaticMeshComponent组件，用于放置关卡物件。
然后需要在.cpp文件中添加头文件Components/StaticMeshComponent.h
另外，SaveLocation的父类为HitBoxBase，所以HitBoxBase中HitBoxComp可以直接使用

### 虚函数覆写
`virtual void OnHitSphere(class ASphereBase * sphere) override;`
这里的虚函数是到了一个关卡重新设置CurrentStart的位置，CurrentStart是SetPlayerLocation()的参数。

### 功能实现
**功能需求**：遇到新的关卡，将CurrentStart保存为新关卡的碰撞盒子处。
1)声明静态网格体和覆写虚函数。
```
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "SaveMeshComp")
		class UStaticMeshComponent * SaveMeshComp;

public:
	virtual void OnHitSphere(class ASphereBase * sphere) override;
```
2).cpp文件中实例化SaveMeshComp组件，HitBoxComp为其附件；
覆写OnHitSphere，获取当前GameMode转化为TestGameGameModeBase类型，在TestGameGameModeBase类中调用SetCurrentStart()函数，将HitBoxComp当前的位置给CurrentStart
```
ASaveLocation::ASaveLocation()
{
	PrimaryActorTick.bCanEverTick = true;
	//实例化SaveMeshComp组件，HitBoxComp为其附件
	SaveMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("SaveMeshComp"));
	HitBoxComp->SetupAttachment(SaveMeshComp);
	
}
void ASaveLocation::OnHitSphere(ASphereBase * sphere)
{
	//获取当前GameMode转化为TestGameGameModeBase类型
	ATestGameGameModeBase * GameModeBase = Cast<ATestGameGameModeBase>(GetWorld()->GetAuthGameMode());
	//TestGameGameModeBase类中调用SetCurrentStart()函数，将HitBoxComp当前的位置给CurrentStart
	GameModeBase->SetCurrentStart(HitBoxComp->GetComponentLocation());
}
```

## 碰撞子类--结束关卡(SaveLocation)
### 添加静态网格体
采用UPROPERTY()声明UStaticMeshComponent组件，用于放置关卡物件。
然后需要在.cpp文件中添加头文件Components/StaticMeshComponent.h
另外，SaveLocation的父类为HitBoxBase，所以HitBoxBase中HitBoxComp可以直接使用

### 虚函数覆写
`virtual void OnHitSphere(class ASphereBase * sphere) override;`
这里的虚函数是设置IsInput的为fasle，取消小球的控制权

### 功能实现
**功能需求**：关卡结束，取消小球的控制权。
1)声明静态网格体和覆写虚函数。
```
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		class UStaticMeshComponent * EndMeshComp;
public:
	virtual void OnHitSphere(class ASphereBase * sphere) override;
```
2).cpp文件中实例化EndMeshComp组件，HitBoxComp为其附件；
覆写OnHitSphere，获取当前GameMode转化为TestGameGameModeBase类型，从TestGameGameModeBase类中调用SetPlayerInput函数，将IsInput设置为fasle，取消玩家控制权
```
AEndLocation::AEndLocation() 
{
	PrimaryActorTick.bCanEverTick = true;
	//实例化EndMeshComp组件，HitBoxComp为其附件
	EndMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("EndMeshComp"));
	HitBoxComp->SetupAttachment(EndMeshComp);
}

void AEndLocation::OnHitSphere(ASphereBase * sphere)
{
	//获取当前GameMode转化为TestGameGameModeBase类型
	ATestGameGameModeBase * GameModeBase = Cast<ATestGameGameModeBase>(GetWorld()->GetAuthGameMode());
	//从TestGameGameModeBase类中调用SetPlayerInput函数，将IsInput设置为fasle，取消玩家控制权
	GameModeBase->SetPlayerInput(false);

}

```
## 游戏模式与功能(GameMode)
**功能需求**将一些公共经常使用的函数写在这里。
这里有三个功能函数，回到指定位置、设置IsInput和CurrentStart
### 功能实现
1)声明变量和函数，
```
public:
	//声明玩家类变量
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "PlayPawn")
		class ASphereBase * PlayPawn;
	//声明初始点坐标
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "CurrentStart")
		FVector CurrentStart;
	//声明死亡次数变量
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "PlayerDieNumber")
		int32 PlayerDieNumber;
	//声明是否结束变量
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		bool IsEnd;

public:
	virtual void BeginPlay() override;

public:
	virtual void Tick(float DeltaTime) override;
	//回到指定点的函数
	UFUNCTION(BlueprintCallable)
	void SetPlayerLocation();
	//设置Isput变量
	UFUNCTION(BlueprintCallable)
	void SetPlayerInput(bool isInput);
	//设置CurrentStart变量
	UFUNCTION(BlueprintCallable)
	void SetCurrentStart(FVector Location);
```
2)相关变量初始化；将获取场景中被控制的Pawn，将其转换为ASphereBase类存储到Pawn中,获取其位置，并赋值给CurrentStart。
这里用到了ASphereBase类，需要添加SphereBase.h文件

```
ATestGameGameModeBase::ATestGameGameModeBase() 
{
	PrimaryActorTick.bCanEverTick = true;	
	PlayerDieNumber = 0;
	IsEnd = false;
}
void ATestGameGameModeBase::BeginPlay()
{
	Super::BeginPlay();
	//将获取场景中被控制的Pawn，将其转换为ASphereBase类存储到Pawn中
	ASphereBase *Pawn = Cast<ASphereBase>(GetWorld()->GetFirstPlayerController()->GetPawn());
	//如果获取到了被控制的Pawn，获取其位置，并赋值给CurrentStart
	if (Pawn) 
	{
		PlayPawn = Pawn;
		CurrentStart = PlayPawn->GetActorLocation();
	}
}
```
3)函数功能定义
```
void ATestGameGameModeBase::SetPlayerLocation()
{
	//根据已经获取的CurrentStart，设置pawn的位置，并将速度归零，死亡次数加一
	PlayPawn->SetActorLocation(CurrentStart);
	PlayPawn->SphereMeshComp->SetPhysicsLinearVelocity(FVector(0, 0, 0));
	PlayPawn->SphereMeshComp->SetPhysicsAngularVelocity(FVector(0, 0, 0));
	PlayerDieNumber++;
}

void ATestGameGameModeBase::SetPlayerInput(bool isInput) 
{	
	//获取当前玩家的IsInput值
	PlayPawn->IsInput = isInput;
	//如果isInput为false
	if (!isInput)
	{
		IsEnd = true;
	}
}

void ATestGameGameModeBase::SetCurrentStart(FVector Location)
{
	//如果isInput为false
	if (Location != FVector(0,0,0))
	{
		CurrentStart = Location;
	}
}
```
## 结束UI
### 强制转换
cast to 指定蓝图 + get游戏相关变量作为object，可以作为相当方便的蓝图通信
### UI bind
UI中的visible和text等可以bind绑定蓝图。
按键可以绑定 Onclick


## .和::和：和->的区别
在学习C++的过程中我们经常会用到.和::和：和->，在此整理一下这些常用符号的区别。 <br>
1)A.B则A为对象或者结构体；<br>
2)A->B则A为指针，->是成员提取，A->B是提取A中的成员B，A只能是指向类、结构、联合的指针；<br>
3)::是作用域运算符，A::B表示作用域A中的名称B，A可以是名字空间、类、结构；<br>
4)：一般用来表示继承； 如class TESTGAME_API ASphereBase : public APawn

## C++中的Overload、Override和Overwrite
在C++语言中有一组基础的概念一直都容易混淆：Overload、Override和Overwrite分别表示什么意思？下面把这三个概念整理一下：

### 1. Overload（重载）
重载的概念最好理解，在同一个类声明范围中，定义了多个名称完全相同、参数（类型或者个数）不相同的函数，就称之为Overload（重载）。重载的特征如下：<br>
1)相同的范围（在同一个类中）；<br>
2)函数名字相同；<br>
3)参数不同；<br>
4)virtual 关键字可有可无。

### 2. Override（覆盖）
覆盖的概念其实是用来实现C++多态性的，即子类重新改写父类声明为virtual的函数。Override（覆盖）的特征如下：<br>
1)不同的范围（分别位于派生类与基类）；<br>
2)函数名字相同；<br>
3)参数列表完全相同；<br>
4)基类函数必须有virtual 关键字。

### 3. Overwrite（改写）
改写是指派生类的函数屏蔽（或者称之为“隐藏”）了与其同名的基类函数。正是这个C++的隐藏规则使得问题的复杂性陡然增加，这里面分为两种情况讨论：<br>
1)如果派生类的函数与基类的函数同名，但是参数不同。那么此时，不论有无virtual关键字，基类的函数将被隐藏（注意别与重载混淆）。<br>
2)如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual关键字。那么此时，基类的函数被隐藏（注意别与覆盖混淆）。

## 杂项总结
1)类型转换<br>
FString::FromInt：UE4中将int类型转化为String类型<br>
FString::SanitizeFloat： UE4中将float类型转化为String类型<br>
2)Object和Actor区别，Actor能挂载组件，Object不能挂载组件<br>
3)Component组件是类的成员，使用前面加U，例如UStaticMeshComponent，静态网格体组件前面加U;UInputComponent，输入组件前面加U<br>
4）Ctrl + Shift + B 不运行，只编译<br>
5)为了避免两个头文件互相引用，一般不在.h文件中导入太多的别的头文件，<br>
如果你只是想在另外一个类中定义一个类的成员变量，只要在头文件中加入 class 类名;<br>
然后要在.cpp文件中包含这个头文件就可以了<br>
6)Bug：** 添加新的头文件时，要将头文件放在.generated.h之前，不然可能出错**<br>
7)有时候改写C++类了以后，其蓝图类需要重新继承，才会有变化。<br>
8)采用UPROPETRTY()声明了某个类的成员，一般在命名时在后面加上组件属性，如添加组件，可以起名SphereMeshComp，其中Comp说明其是组件。<br>
9)OnCollision 可以自定义的碰撞函数，名称可以任意，但参数形式必须满足OnComponentBeginOverlap定义的参数，有六个参数，平常很难记住这么多参数，不知道的函数可以去F12看源码，多看多积累
10)有时候用C++写好类，继承到蓝图时，有时改代码不生效，删除蓝图重新继承一下可能会有效果。
11)蓝图通信，引用(直接使用变量)大于接口大于强转大于GetAllactorOfClass
