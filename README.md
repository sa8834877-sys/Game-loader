# Game-loader
This is best website for downloading game and files this website made in Pakistan 
#include "OpenWorldHero.h"
#include "GameFramework/SpringArmComponent.h"
#include "Camera/CameraComponent.h"
#include "GameFramework/Controller.h"
#include "Components/InputComponent.h"

// Constructor: Setup Character
AOpenWorldHero::AOpenWorldHero()
{
    // Controller Rotation settings (Smoothness ke liye)
    bUseControllerRotationPitch = false;
    bUseControllerRotationYaw = false;
    bUseControllerRotationRoll = false;

    // Camera Boom setup (Character ke piche camera latkane ke liye)
    CameraBoom = CreateDefaultSubobject<USpringArmComponent>(TEXT("CameraBoom"));
    CameraBoom->SetupAttachment(RootComponent);
    CameraBoom->TargetArmLength = 400.0f; // Camera kitna door rahega
    CameraBoom->bUsePawnControlRotation = true; 

    // Camera setup
    FollowCamera = CreateDefaultSubobject<UCameraComponent>(TEXT("FollowCamera"));
    FollowCamera->SetupAttachment(CameraBoom, USpringArmComponent::SocketName);
    FollowCamera->bUsePawnControlRotation = false;

    // Sensitivity Settings (HD Controller feel)
    BaseTurnRate = 45.f;
    BaseLookUpRate = 45.f;
}

void AOpenWorldHero::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
    Super::SetupPlayerInputComponent(PlayerInputComponent);

    // --- Joystick Mappings ---
    // Left Stick (Movement)
    PlayerInputComponent->BindAxis("MoveForward", this, &AOpenWorldHero::MoveForward);
    PlayerInputComponent->BindAxis("MoveRight", this, &AOpenWorldHero::MoveRight);

    // Right Stick (Camera)
    PlayerInputComponent->BindAxis("Turn", this, &APawn::AddControllerYawInput);
    PlayerInputComponent->BindAxis("LookUp", this, &APawn::AddControllerPitchInput);

    // Buttons (Jump)
    PlayerInputComponent->BindAction("Jump", IE_Pressed, this, &ACharacter::Jump);
    PlayerInputComponent->BindAction("Jump", IE_Released, this, &ACharacter::StopJumping);
}

void AOpenWorldHero::MoveForward(float Value)
{
    if ((Controller != nullptr) && (Value != 0.0f))
    {
        // Pata karo ki "Saamne" kaunsi disha hai
        const FRotator Rotation = Controller->GetControlRotation();
        const FRotator YawRotation(0, Rotation.Yaw, 0);

        // Direction calculate karo
        const FVector Direction = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::X);
        
        // Character ko move karo
        AddMovementInput(Direction, Value);
    }
}

void AOpenWorldHero::MoveRight(float Value)
{
    if ((Controller != nullptr) && (Value != 0.0f))
    {
        // Pata karo ki "Right" kaunsi disha hai
        const FRotator Rotation = Controller->GetControlRotation();
        const FRotator YawRotation(0, Rotation.Yaw, 0);
    
        const FVector Direction = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::Y);
        AddMovementInput(Direction, Value);
    }
}
DOWNLOAD
