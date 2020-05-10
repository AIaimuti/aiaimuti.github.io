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
