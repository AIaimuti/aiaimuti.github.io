---
layout:     post
title:      蒙太奇动画
subtitle:   基础内容
date:       2020-5-6
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

<iframe src="//player.bilibili.com/player.html?aid=540566111&bvid=BV1Ci4y1x7Rv&cid=187934977&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
今天要实现的功能是按攻击键，随机播放不同攻击动画

## 蒙太奇动画
蒙太奇动画有个插槽，插槽是管理蒙太奇动画的，输入连接了装有蒙太奇动画的插槽就相当于可以控制蒙太奇动画播放了

### 蒙太奇动画制作
今天制作的蒙太奇器动画由两段构成，<br>
先将我们所需的动画托到插槽放入插槽,但是这样动画是连续播放的；<br>
因此右键插槽处新建一个片段Attack2，并把原来的片段命名为Attack1；<br>
点击clear或者X将下方片段分开<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Montage_1.png?raw=true)<br>
然后打开动画蓝图，在最顶层状态机后面添加 Slot‘DefaultSlot’ ，这样状态机和蒙太奇就重叠在一起了<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Montage_2.png?raw=true)<br>

### 人物类中修改代码
声明了一个蒙太奇变量；<br>
用于记录点击的变量和函数和记录攻击的变量和函数；
```
public:
	//声明蒙太奇变量
	UPROPERTY(EditDefaultsOnly, BlueprintReadOnly)
		class UAnimMontage* AnimMontage;
	//记录点击的变量和函数
	bool bClicking;

	UFUNCTION()
		void OnClickBegin();

	UFUNCTION()
		void OnClickEnd();
	//记录攻击的变量和函数
	bool bAttacking;

	void AttackBegin();

	UFUNCTION(BlueprintCallable)
		void AttackEnd();
```
### 人物类中函数实现
先改变主角和武器交互的函数，让攻击和武器装备不能同时进行
```
oid AMan::OnInteract()
{
	//如果没有进行攻击
	if (!bAttacking)
	{
		//这里的ActiveOverlapItem值是Weapon触发函数里给的
		//遇见了物体尝试转化为武器
		AWeapon *Weapon = Cast<AWeapon>(ActiveOverlapItem);
		//如果成功
		if (Weapon)
		{
			Weapon->Equip(this);
			ActiveOverlapItem = nullptr;
		}
	}
}
```
鼠标点击时，没有正在进行攻击并且装备了武器就可以进行攻击
```
void AMan::OnClickBegin()
{
	bClicking = true;	
	//如果没有开始攻击并装备了武器
	if (!bAttacking && EquippedWeapon)
	{
		AttackBegin();
	}
}
```
鼠标结束就可以将bClicking设为false
```
void AMan::OnClickEnd()
{
	bClicking = false;
}
```
攻击函数，bAttacking设为true
首先获取蒙太奇实例，然后判断是否有蒙太奇动画、有动画实例并且蒙太奇攻击动画没有执行；
如果是，动画实例播放蒙太奇动画，
然后采用布尔随机数，进行攻击动画的随机播放。
```
void AMan::AttackBegin()
{
	bAttacking = true;
	//拿到动画实例
	UAnimInstance *Instance = GetMesh()->GetAnimInstance();
	//如果有蒙太奇动画、有动画实例并且蒙太奇攻击动画没有执行
	if (AnimMontage && Instance && !Instance->Montage_IsPlaying(AnimMontage))
	{
		//实例播放蒙太奇动画
		Instance->Montage_Play(AnimMontage);
		if (FMath::RandBool())
		{
			Instance->Montage_JumpToSection(FName("Attack1"), AnimMontage);
		}
		else
		{
			Instance->Montage_JumpToSection(FName("Attack2"), AnimMontage);
		}
		
	}
}
```
攻击结束bAttacking为false
如果鼠标一直按住，执行连续攻击
```
void AMan::AttackEnd()
{
	bAttacking = false;
	//如果鼠标一直按住，执行连续攻击
	if (bClicking && EquippedWeapon)
	{
		AttackBegin();
	}
}
```
项目设置中添加鼠标映射，在这里绑定
```
// Called to bind functionality to input
void AMan::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
///////////////////////////////////其它省略///////////////////////////////
	//绑定鼠标攻击
	PlayerInputComponent->BindAction("Click", IE_Pressed, this, &AMan::OnClickBegin);
	PlayerInputComponent->BindAction("Click", IE_Released, this, &AMan::OnClickEnd);
}
然后在人物蓝图中，选取我们自定义的蒙太奇动画
```
### 攻击结束蓝图实现
其实到这里，上述的攻击部分还有一些问题，攻击一次就不执行攻击了，因为没有进入AttackEnd的条件<br>
在程序里检测攻击的结束其实比较困难，因此我们在蒙太奇里添加一个结束表示用于表示动画结束<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Montage_3.png?raw=true)<br>
然后在动画蓝图中使用蒙太奇结束事件，调用AttackEnd函数，这样就可以了<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/UE4/UE4_C++/Montage_3.png?raw=true)

