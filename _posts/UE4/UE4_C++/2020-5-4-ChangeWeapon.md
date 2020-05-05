---
layout:     post
title:      切换武器
subtitle:   基础内容
date:       2020-5-4
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

<iframe src="//player.bilibili.com/player.html?aid=753065899&bvid=BV17k4y1k7zs&cid=186989994&page=1" width="720" height="480" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> <br>

今天要做的是武器拾取的切换，一般游戏是拾取了武器到背包或者装备栏，目前是按E拾取武器销毁，并前面拾取的武器。

## 切换武器
在上次拾取武器的类Weapon上,进行一些改动，主要改动为两方面<br>
1.对主角Man关于获取武器相关信息的改动<br>
2.对Wdeapon的触发和离开触发相关内容的修改

### 对主角Man的改动准备
这里定义了一个AItem的指针，用于触发时指针指向待拾取的武器；<br>
OnInteract是与待拾取武器互动的函数；<br>
SetWeapon记录当前主角的武器持有的武器，再次拾取会覆盖前一个武器，由EquiqedWeapon变量保存当前武器。<br>
EquiqedWeapon之所以写在protected里是因为写在public里十分麻烦，还需要很多其他操作
```
public:
	//触发时指针指向待拾取的武器
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
		class AItem* ActiveOverlapItem;
	//与待拾取武器互动的函数，按E调用
	UFUNCTION()
		void OnInteract();
	//设置武器，返回被捡去信息
	void SetWeapon(class AWeapon *Weapon);

protected:
	//记录当前武器的变量
	UPROPERTY(EditDefaultsOnly, BlueprintReadOnly)
		class AWeapon* EquiqedWeapon;
```
### 函数实现
OnInteract与按键E绑定，按下E时，将ActiveOverlapItem转化为Weapon；<br>
然后调用Weapon中的Equip函数，this就指代Weapon，装备后，清空指针ActiveOverlapItem。<br>
这里的ActiveOverlapItem值是Weapon触发函数里给的
```
void AMan::OnInteract()
{
	//这里的ActiveOverlapItem值是Weapon触发函数里给的
	AWeapon *Weapon = Cast<AWeapon>(ActiveOverlapItem);
	if (Weapon)
	{	
		Weapon->Equip(this);
		ActiveOverlapItem = nullptr;
	}
}
void AMan::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	/////////////省略其它内容////////////////
	//与武器互动,绑定按键E
	PlayerInputComponent->BindAction("Interact", IE_Released, this, &AMan::OnInteract);
}
```
SetWeapon则是用来装备和替换武器，EquiqedWeapon来记录当前持有的武器信息；<br>
拾取时如果有武器，则销毁当前武器，放入新的武器，这是武器自身去调用的
```
void AMan::SetWeapon(AWeapon * Weapon)
{
	//如果已经有武器，则销毁当前武器
	if (EquiqedWeapon)
	{
		EquiqedWeapon->Destroy();
	}
	//装备新的武器
	EquiqedWeapon = Weapon;
}
```

### 对Weapon类的改动准备
这里加入了离开触发的函数的覆写和武器装备的声音，以及装备武器的函数
```
//离开触发的函数
virtual void OnOverlapEnd_Item(UPrimitiveComponent* OverlappedComponent,
	AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)override;
//武器装备时的声音
UPROPERTY(EditAnywhere, BlueprintReadOnly)
	USoundCue *SoundEquiped;

void Equip(class AMan *Man);
```
### Weapon类切换武器函数实现
Weapon.cpp添加头文件：<br>
//声音控制<br>
#include "Sound/SoundCue.h"<br>
//播放声音用的<br>
#include "Kismet/GameplayStatics.h"<br>
//用于打印的<br>
#include "Engine/Engine.h"<br>

这里首先修改了OnOverlapBegin_Item函数；<br>
当拿到主角实例时，先对ActiveOverlapItem进行一些逻辑处理；<br>
ActiveOverlapItem是一个指向待拾取武器的指针，但当同时接触多个武器时ActiveOverlapItem会有一些混乱；<br>
因此，在这里当遇到待拾取武器时，先让ActiveOverlapItem指向遇到的第一个武器；<br>
这样使逻辑简化，再接触其它物体时会忽略，这样就只能捡起一把碰到的武器<br>

然后使用了TArray类，TArray是UE4的一个数组模版；<br>
`TArray <FStringFormatArg> FormatArray;`中的FStringFormatArg类型，会将输入的字符串变成索引的形式以方便运用;<br>
FormatArray获取组件名称放在索引{0};<br>
使用AddOnScreenDebugMessage将FormatArray索引{0}的内容显示在屏幕上
```
void AWeapon::OnOverlapBegin_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult & SweepResult)
{
	Super::OnOverlapBegin_Item(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex, bFromSweep, SweepResult);
	if (OtherActor)
	{
		//拿到碰撞触发的实例
		AMan * Man = Cast<AMan>(OtherActor);
		//对实例装备武器
		if (Man)
		{
			//这里首先修改了OnOverlapBegin_Item函数；
			//当拿到主角实例时，先对ActiveOverlapItem进行一些逻辑处理；
			//ActiveOverlapItem是一个指向待拾取武器的指针，但当同时接触多个武器时ActiveOverlapItem会有一些混乱；
			//因此，在这里当遇到待拾取武器时，先让ActiveOverlapItem指向Man自身的武器；
			//这样使逻辑简化，再接触其它物体时会忽略，这样就只能捡起一把碰到的武器
			if (Man->ActiveOverlapItem == nullptr)
			{
				Man->ActiveOverlapItem = this;
			}
			//显示在屏幕上的相关配置
			//定义FStringFormatArg类的FormatArray
			TArray <FStringFormatArg> FormatArray;
			//FormatArray获取组件名称放在索引{0}
			FormatArray.Add(GetName());
			//使用AddOnScreenDebugMessage将FormatArray索引{0}的内容显示在屏幕上
			GEngine->AddOnScreenDebugMessage(-1, 100, FColor::Blue,
				FString::Format(TEXT("Press E to equip {0}"),FormatArray), true, FVector2D(4,4));

			//Equip(Man);
		}
	}
}
```
当离开第一个释放ActiveOverlapItem指针，使其为空;<br>
这里也有问题，当我们同时接触两个物体时，离开了第一个物体但没离开第二个物体时，<br>
ActiveOverlapItem也会为空，这会导致第二个物体捡不到;<br>
这里为了使逻辑简单，先这样写
```
void AWeapon::OnOverlapEnd_Item(UPrimitiveComponent * OverlappedComponent, AActor * OtherActor, UPrimitiveComponent * OtherComp, int32 OtherBodyIndex)
{
	Super::OnOverlapEnd_Item(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex);
	if (OtherActor)
	{
		AMan *Man = Cast<AMan>(OtherActor);
		//这里也有问题，当我们同时接触两个物体时，离开了第一个物体但没离开第二个物体时，
		//ActiveOverlapItem也会为空，这会导致第二个物体捡不到
		if (Man->ActiveOverlapItem == this)
		{
			Man->ActiveOverlapItem = nullptr;
		}
		//消除屏幕上的文字
		GEngine->ClearOnScreenDebugMessages();
	}
}
```
装备函数，将武器对于所有通道的碰撞设置为忽略并关闭模拟物理
这里较上一次添加了解绑定碰撞触发离开函数，避免一直调用触发函数功能
然后播放装备声音，设置武器

```
void AWeapon::Equip(AMan * Man)
{
	if (Man)
	{
		//把武器对于所有通道的碰撞设置为忽略
		Mesh->SetCollisionResponseToAllChannels(ECR_Ignore);
		//设置武器模拟物理为false
		Mesh->SetSimulatePhysics(false);
		//把武器附着在主角身上的函数，三个参数
		//第一个是获取Pawn的网格，第二个是附着的方式，第三个是插槽的名字
		//FAttachmentTransformRules有四种方式
		//保持相对位置KeepRelativeTransform; 保持世界位置KeepWorldTransform; 
		//附着不包括缩放SnapToTargetNotIncludingScale; 附着包括缩放SnapToTargetIncludingScale;
		AttachToComponent(Man->GetMesh(), FAttachmentTransformRules::SnapToTargetNotIncludingScale, "WeaponSocket");
		Rotate = false;
		//关闭粒子特效
		ParticleComp->SetActive(false);
		//解绑定碰撞触发函数，避免一直调用触发函数功能
		SphereComp->OnComponentBeginOverlap.RemoveDynamic(this, &AItem::OnOverlapBegin_Item);
		//解绑定碰撞触发离开函数，避免一直调用触发函数功能
		SphereComp->OnComponentEndOverlap.RemoveDynamic(this, &AItem::OnOverlapEnd_Item);
		GEngine->ClearOnScreenDebugMessages();
		//播放装备声音
		if (SoundEquiped)
		{
			UGameplayStatics::PlaySound2D(this, SoundEquiped);
		}
		//装备武器
		Man->SetWeapon(this);
	}
}
```
整体思路就是主角碰到武器，把武器用ActiveOverlapItem临时记下，<br>
如果按E则调用OnInteract函数与Weapon互动，调用Weapon的Equip函数；<br>
Equip函数调用主角的SetWeapon函数，告诉主角自己被装备了，记录下当前装备的武器。
