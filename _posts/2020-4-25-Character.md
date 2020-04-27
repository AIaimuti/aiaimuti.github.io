---
layout:     post
title:      Character
subtitle:   基础内容
date:       2020-4-25
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - UE4 C++入门系列
---

## Character摄像机和弹簧臂
### 摄像机和弹簧臂配置
1).h文件
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
  class USpringArmComponent * SpringArmComp;

UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
  class UCameraComponent * Camera;
```
2).cpp文件
添加头文件：
#include "GameFramework/SpringArmComponent.h"<br>
#include "Camera/CameraComponent.h"<br>
这里注意的是Character有控制器，一般让弹簧臂跟着控制器转<br>
但摄像机不要跟着控制器转
```
//实例化弹簧臂组件
SpringArmComp = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArmComp"));
//弹簧臂组件组件附着在根组件
SpringArmComp->SetupAttachment(RootComponent);
//弹簧臂组件长度设为400
SpringArmComp->TargetArmLength = 600;
//使用控制器，弹簧臂跟着控制器旋转，以控制器的旋转为准
SpringArmComp->bUsePawnControlRotation = true;
//摄像机放在弹簧臂插槽
Camera = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));
Camera->SetupAttachment(SpringArmComp, USpringArmComponent::SocketName);
//不要让摄像机跟着控制器转
Camera->bUsePawnControlRotation = false;
```
## Character运动
### Character运动配置
1).h文件
```
//视角左右旋转速率
float BaseTurnRate;
//视角上下旋转速率
float BaseLookupRate;
//前后移动函数
void MoveForward(float Value);
//左右移动函数
void MoveRight(float Value);
//
void TurnAtRate(float Value);

void TurnLookupRate(float Value);
```
2).cpp文件<br>
添加头文件：<br>
#include "Engine/World.h"<br>
#include "Components/InputComponent.h"<br>
#include "GameFramework/Controller.h"<br>
#include "GameFramework/CharacterMovementComponent.h"<br>
我们调整视角不希望身体转动，视角和控制器绑定，所以要让身体不跟着控制器旋转
因此bUseControllerRotation相关设置为false
```
//设置上下和左右调整的速率
BaseTurnRate = 65;
BaseLookupRate = 65;
//我们调整视角不希望身体转动，视角和控制器绑定，所以要让身体不跟着控制器旋转
bUseControllerRotationYaw = false;
bUseControllerRotationPitch = false;
bUseControllerRotationRoll = false;
//角色会随着移动的方向而转身
GetCharacterMovement()->bOrientRotationToMovement = true;
//影响转速速率，第二个参数沿z轴转，是yaw值
GetCharacterMovement()->RotationRate = FRotator(0.f, 560, 0);
//向上跳的速率
GetCharacterMovement()->JumpZVelocity = 650;
//腾空之后对跳跃的横向控制量
GetCharacterMovement()->AirControl = 0.2f;
```
### Character运动函数实现
Character内置一些写好的运动函数，这里轴/按键映射可以直接绑定如&ACharacter::Jump、&ACharacter::StopJumping<br>
摄像机弹簧臂和控制器绑定，因此将CameraPitch绑定&APawn::AddControllerPitchInput、CameraYaw绑定&APawn::AddControllerYawInput<br>
这样，鼠标输入的上下左右绑定控制器，控制器与摄像机弹簧臂绑定，就形成了鼠标控制弹簧臂进而调整视角。<br>
Lookup和Turn绑定&AMan::TurnLookupRate和&AMan::TurnAtRate，函数内容是官方例子实现，效果与上述相同。
```
void AMan::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	//ACharacter写好的跳跃，直接调用
	PlayerInputComponent->BindAction("Jump",IE_Pressed, this, &ACharacter::Jump);
	PlayerInputComponent->BindAction("Jump", IE_Released, this, &ACharacter::StopJumping);
	//绑定前后、左右移动函数和摄像机移动(即控制器移动的输入)
	PlayerInputComponent->BindAxis("MoveForward", this, &AMan::MoveForward);
	PlayerInputComponent->BindAxis("MoveRight", this, &AMan::MoveRight);
	PlayerInputComponent->BindAxis("CameraPitch", this, &APawn::AddControllerPitchInput);
	PlayerInputComponent->BindAxis("CameraYaw", this, &APawn::AddControllerYawInput);
	PlayerInputComponent->BindAxis("Lookup", this, &AMan::TurnLookupRate);
	PlayerInputComponent->BindAxis("Turn", this, &AMan::TurnAtRate);
}
```
FRotationMatrix(YawRotation)描述了一个基于旋转量YawRotation旋转过后的空间,<br>
然后从旋转后的空间找到当前X方向的单位向量，即是旋转后前方<br>
然后使用AddMovementInput在前方施加一个Value大小的运动量。<br>
AddMovementInput实质上是对Pawn的MovementComponent组件进行控制
```
void AMan::MoveForward(float Value)
{
	//如果有控制器和Value输入
	if (Controller && Value != 0)
	{	//获取控制器旋转
		const FRotator Rotation = Controller->GetControlRotation();
		//以控制器旋转作为左右旋转的值
		const FRotator YawRotation(0.f, Rotation.Yaw, 0);
		//FRotationMatrix描述了一个基于旋转量YawRotation旋转过后的空间，然后输出当前X方向的单位向量，即是旋转后前方
		const FVector Direction = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::X);
		//在前方施加一个Value大小的运动量，对MovementComponent运动组件进行控制
		AddMovementInput(Direction, Value);
	}

}

void AMan::MoveRight(float Value)
{
	//如果有控制器和Value输入
	if (Controller && Value != 0)
	{	//获取控制器旋转
		const FRotator Rotation = Controller->GetControlRotation();
		//以控制器旋转作为左右旋转的值
		const FRotator YawRotation(0.f, Rotation.Yaw, 0);
		//FRotationMatrix描述了一个基于旋转量旋转过后的空间，然后输出当前Y方向的单位向量，即是旋转后右方
		const FVector Direction = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::Y);
		//在右方施加一个Value大小的运动量，对MovementComponent运动组件进行控制
		AddMovementInput(Direction, Value);
	}
}
void AMan::TurnAtRate(float Value)
{
	//左右方向的输入量Value * 旋转速率 * 旋转时间 = 左右旋转的角度
	AddControllerYawInput(Value * BaseTurnRate * GetWorld()->DeltaTimeSeconds);
}

void AMan::TurnLookupRate(float Value)
{
	//上下方向的输入量Value * 旋转速率 * 旋转时间 = 上下旋转的角度
	AddControllerPitchInput(Value * BaseLookupRate * GetWorld()->DeltaTimeSeconds);
}
```
