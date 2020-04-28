---
layout:     post
title:      动画蓝图
subtitle:   基础内容
date:       2020-4-25
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---
## 蓝图动画
1)创建一个继承自AnimInstance的C++类<br>
2).h文件中声明
```
//定义MovementSpeed为移动速度
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
	float MovementSpeed;
//IsJumping判断是否在跳跃
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
	bool IsJumping;
//定义Pawn用于获取一些玩家的信息
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Movement)
	class APawn * Pawn;
//原生函数，用于初始化
virtual void NativeInitializeAnimation() override;
//用于更新一些动画的属性
UFUNCTION(BlueprintCallable, Category = AnimationProperty)
	void UpdateAnimationProperties();
```
3).cpp文件中定义
```
void UMainAnimInstance::NativeInitializeAnimation()
{
	//获取pawn的拥有者
	if (!Pawn)
	{
		Pawn = TryGetPawnOwner();
	}
}

void UMainAnimInstance::UpdateAnimationProperties()
{
	//获取pawn的拥有者
	if (!Pawn)
	{
		Pawn = TryGetPawnOwner();
	}

	if (Pawn)
	{
		//获取pawn的输入速度
		FVector Speed = Pawn->GetVelocity();
		//只使用Speed X、Y方向的速度
		FVector LateralSpeed = FVector(Speed.X, Speed.Y, 0);
		//将速度信息给MovementSpeed
		MovementSpeed = LateralSpeed.Size();
		//检测是否在跳跃，将跳跃信息给IsJumping
		IsJumping = Pawn->GetMovementComponent()->IsFalling();
	}
}
```
### 动画一维混合空间1D/2D
内容浏览器右键-->Animation-->BlendSpace 1D
在一个维度上给多个动画加权形成混合动画，动画是通过输入参数的大小来进行加权
Asset details-->Axis Setting-->Horizontal Axis横轴
Name：名称
min Axis Value：轴最小值
max Axis Value：轴最大值
Number of gird Divitions：分格数量，最多用于动画混合的数量
Interpolation time：插值时间，目前不知道怎么用
Interpolation Typ：插值类型，即采用何种方式加权插值

### 状态机
在动画蓝图动画事件中，右键添加状态机
双击进入状态机，可以建立不同状态的相互转换关系。
不同状态之间必须有转换条件
time remaining (rait0)是一个按前一个动画完成事件来进行转化的
