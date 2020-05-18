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
有了敌人的动画类，然后我们需要一个敌人类去设定追踪目标以及攻击目标的各种条件
