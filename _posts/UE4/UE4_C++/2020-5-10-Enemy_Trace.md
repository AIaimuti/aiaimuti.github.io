---
layout:     post
title:      敌人追踪和攻击
subtitle:   基础内容
date:       2020-5-10
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
<iframe src="//player.bilibili.com/player.html?aid=370665697&bvid=BV1UZ4y1s7ij&cid=192623264&page=1"  width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

今天做一个怪物追踪与攻击的功能

## 敌人追踪和攻击

敌人的自动追踪功能涉及AI的机制与功能，因此先在C++工程中的Build.cs文件中添加AIModule如下
`PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore","UMG", "AIModule" });`
然后创建一个敌人的动画实例EnemyAnimInstance继承于AnimInstance

### 敌人的动画实例准备
覆写动画蓝图初始化函数，以及声明更新蓝图动画的函数
声明运动速度以及蓝图动画的拥有者
```
public:
	//声明一个初始化函数
	virtual void NativeInitializeAnimation() override;
	//声明一个用于更新动画状态的函数
	UFUNCTION(BlueprintCallable,Category = AnimationProperty)
		void UpdateAnimationProperties();
	//运动速度
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
		float MovementSpeed;
	//动画实例的拥有者--敌人
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
		class AEnemy *Enemy;
	//动画实例的拥有者--人类
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
		class APawn *Pawn;
```

### 敌人的动画实例函数
这里和人物动画那里的配置是类似的<br>
初始化函数中将动画实例的拥有者转换为Enemy
```
void UEnemyAnimInstance::NativeInitializeAnimation()
{
	if (!Pawn)
	{
		Pawn = TryGetPawnOwner();
	}
	//把敌人拥有者初始化
	Enemy = Cast<AEnemy>(Pawn);
}
```
因为移动速度是一个我们需要用在状态机的变量，<br>
因此定义一个状态更新函数，实时进行参数的更新和传递
```
void UEnemyAnimInstance::UpdateAnimationProperties()
{
	if (!Pawn)
	{
		Pawn = TryGetPawnOwner();
	}
	//更新敌人的速率
	if (Pawn)
	{
		//获取Pawn的速度
		FVector Speed = Pawn->GetVelocity();
		//获取Pawn的横向速度
		FVector LateralSpeed = FVector(Speed.X, Speed.Y, 0);
		MovementSpeed = LateralSpeed.Size();
	}
}
```

### 敌人类
有了敌人的动画类，然后我们需要一个敌人类去设定追踪目标以及攻击目标的各种条件<br>
敌人的状态目前来说是分攻击、追踪和静止三个状态的，所以我们定义一个枚举变量表示状态<br>
```
//定义枚举类，三个敌人的状态
UENUM(BlueprintType)
enum class EMoveStatus :uint8
{
	//站立追逐和攻击状态
	MS_Idle		  UMETA(DisplayName = "Idle"),
	MS_MoveToTarget   UMETA(DisplayName = "MoveToTarget"),
	MS_Attacking      UMETA(DisplayName = "Attacking")
};
```
该枚举状态需要在类中声明<br>
其攻击状态对应着其攻击范围、攻击目标以及开始和结束攻击的函数<br>
同理，其追踪状态对应着其追踪范围、追踪目标以及追踪开始和结束的函数<br>
另外，敌人是通过AI模块追踪的，所以也需要声明AI控制器以及追踪函数
```
public:
	// Sets default values for this character's properties
	AEnemy();
	//声明敌人的侦查范围和攻击范围
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		class USphereComponent * DetectSphere;
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		class USphereComponent * AttackSphere;

	UPROPERTY(VisibleAnywhere, BlueprintReadWrite)
		EMoveStatus MoveStatus;
	//AI控制器，因为追踪玩家时需要导航
	class AAIController *AIController;
	//记录敌人追踪时的目标
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		class AMan * TargetMan;
	//记录敌人攻击时的目标
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		class AMan * HittingMan;

	//检测时的重合和离开函数
	UFUNCTION()
	virtual void OnDetectOverlapBegin(UPrimitiveComponent* OverlappedComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComp,
		int32 OtherBodyIndex, bool bFromSweep,
		const FHitResult & SweepResult);
	UFUNCTION()
	virtual void OnDetectOverlapEnd(UPrimitiveComponent* OverlappedComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex);

	//攻击时的重合和离开函数
	UFUNCTION()
		virtual void OnAttackOverlapBegin(UPrimitiveComponent* OverlappedComponent,
			AActor* OtherActor, UPrimitiveComponent* OtherComp,
			int32 OtherBodyIndex, bool bFromSweep,
			const FHitResult & SweepResult);
	UFUNCTION()
		virtual void OnAttackOverlapEnd(UPrimitiveComponent* OverlappedComponent,
			AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex);
	//向目标移动的函数
	UFUNCTION(BlueprintCallable)
		void MoveToTarget();
```
### 敌人类实现
首先在初始化函数中对两个范围初始化
```
AEnemy::AEnemy()
{
 	// Set this character to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
	//侦测范围初始化
	DetectSphere = CreateDefaultSubobject<USphereComponent>(TEXT("DetectSphere"));
	DetectSphere->SetupAttachment(RootComponent);
	DetectSphere->SetSphereRadius(600);
	//攻击范围初始化
	AttackSphere = CreateDefaultSubobject<USphereComponent>(TEXT("AttackSphere"));
	AttackSphere->SetupAttachment(RootComponent);
	AttackSphere->SetSphereRadius(300);

}
```
然后设置追击开始函数,目前的追踪函数只追踪一个目标;<br>
将进入范围Actor进行类型转换为Man，如果成功，则将Man设为TargetMan;<br>
另外设置在攻击时不能追踪，如果没有在攻击状态，则将状态设置为追击，<br>
然后调用追击函数，追击函数只在状态为追击时才有效
```
void AEnemy::OnDetectOverlapBegin(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	//如果没有追踪对象，则获取；有追踪对象则不执行，相当于只追踪一个目标
	if (!TargetMan)
	{
		if (OtherActor)
		{
			AMan *Man = Cast<AMan>(OtherActor);
			if (Man)
			{
				TargetMan = Man;
				//攻击状态时播放动画的状态，在播放状态不可以追踪
				if (MoveStatus != EMoveStatus::MS_Attacking)
				{
					//如果不是攻击状态，进行追击
					MoveStatus = EMoveStatus::MS_MoveToTarget;
				}
				MoveToTarget();
			}
		}
	}
}
```
然后是离开追击范围的函数，这里的逻辑做了简化，并没有设置追踪第二个人；<br>
如果离开范围的是追踪目标且没有攻击，则将状态设置为站立<br>
并停止AI控制器的自动追踪
```
void AEnemy::OnDetectOverlapEnd(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	if (OtherActor)
	{
		AMan *Man = Cast<AMan>(OtherActor);
		//如果离开的对象是被追踪的对象
		if (Man == TargetMan)
		{
			//清空被追踪的对象
			TargetMan = nullptr;
			if (MoveStatus != EMoveStatus::MS_Attacking)
			{
				//如果不是在攻击，就变成站立
				MoveStatus = EMoveStatus::MS_Idle;
			}
			if (AIController)
			{
				//AI控制器停止追踪
				AIController->StopMovement();
			}
		}
	}
}
```
攻击范围在追击范围之内，当进入攻击范围时，敌人状态进入攻击状态；<br>
开始攻击时停止追击
```
void AEnemy::OnAttackOverlapBegin(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	//如果没有攻击对象，则获取；有追踪对象则不执行，相当于只追踪一个目标
	if (!HittingMan)
	{
		if (OtherActor)
		{	//如果没有攻击的目标，把当前进入范围的Pawn转化为HittingMan
			AMan *Man = Cast<AMan>(OtherActor);
			if (Man)
			{
				HittingMan = Man;
				//如果有玩家则无条件得进入攻击状态
				MoveStatus = EMoveStatus::MS_Attacking;
				//开始攻击则停止追踪
				if (AIController)
				{
					AIController->StopMovement();
				}
			}
		}
	}
}
```
离开攻击范围时，主角会先进入追踪范围，因此将HittingMan赋值给TargetMan，HittingMan清空；<br>
```
void AEnemy::OnAttackOverlapEnd(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	if (OtherActor)
	{	
		AMan *Man = Cast<AMan>(OtherActor);
		if (Man == HittingMan)
		{
			//离开了攻击范围，把攻击者变成追踪者
			//这里简单处理，不会从攻击状态停止就在攻击范围内找另一个玩家
			TargetMan = HittingMan;
			HittingMan = nullptr;
		}

	}


}
```
然后是追踪时调用的函数MoveToTarget，当有TargetMan时调用AIController模块追踪，否则状态为站立<br>
AIController调用MoveTo函数有两个关键参数，一个是移动请求，一个是路线；<br>
移动请求通过FAIMoveRequest定义，并设置追踪对象为TargetMan，且在TargetMan为圆心半径10范围内，判定为到达，<br>
FNavPathSharedPtr定义追踪路线，配合追踪盒子使用，<br>
需要在场景中设置追踪盒子的范围，且敌人在范围内才能追踪，通常追踪盒子下边缘要在地面之下才能完全包住敌人，敌人才能进行追踪<br>
```
void AEnemy::MoveToTarget()
{
	if (TargetMan)
	{
		if (AIController)
		{
			UE_LOG(LogTemp, Warning, TEXT("%s"), *FString(__FUNCTION__));
			//设置AI移动请求
			FAIMoveRequest MoveRequest;
			//AI移动追踪的目标是TargetMan
			MoveRequest.SetGoalActor(TargetMan);
			//在TargetMan半径10范围内算是追到了
			MoveRequest.SetAcceptanceRadius(10);

			//追踪经过的路径,作为输出可以告诉我们，AI是用什么路径去追踪的
			FNavPathSharedPtr NavPath;
			//AI控制器，施加移动请求
			AIController->MoveTo(MoveRequest, &NavPath);

			//将路径上所有的点用球画出来
#if 0
			auto PathPoints = NavPath->GetPathPoints();
			for (auto Point:PathPoints)
			{
				FVector Location = Point.Location;
				UKismetSystemLibrary::DrawDebugSphere(this, Location);
			}
#endif
		}
	}
	else
	{
		MoveStatus = EMoveStatus::MS_Idle;
	}
}
```
BeginPlay中获取AAIController，并将AI追踪和触发开始结束的函数进行绑定
```
void AEnemy::BeginPlay()
{
	Super::BeginPlay();
	//获取AI控制器
	AIController = Cast<AAIController>(GetController());
	//将AI追踪触发进行函数绑定
	DetectSphere->OnComponentBeginOverlap.AddDynamic(this, &AEnemy::OnDetectOverlapBegin);
	DetectSphere->OnComponentEndOverlap.AddDynamic(this, &AEnemy::OnDetectOverlapEnd);
	//将AI攻击触发进行函数绑定
	AttackSphere->OnComponentBeginOverlap.AddDynamic(this, &AEnemy::OnAttackOverlapBegin);
	AttackSphere->OnComponentEndOverlap.AddDynamic(this, &AEnemy::OnAttackOverlapEnd);
}
```
