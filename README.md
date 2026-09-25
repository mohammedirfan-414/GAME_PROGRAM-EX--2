# GAME_PROGRAM-EX--2
## Create a player movement using character, collectable, player health and score
## Aim
Create a playable third-person character in Unreal Engine that can move and run, collect coin-like collectibles, track a Score and Player Health, and display both on-screen (UI).

## Overview:
Player Character (BP_PlayerCharacter) — handles movement, input, health, and overlaps with collectables.
Collectable (BP_Collectable) — simple actor with collision that gives score (and optionally health) when overlapped.
UI (WBP_HUD) — UMG Widget showing Score and Health values.
GameMode / PlayerState — (optional) hold persistent Score/HighScore across respawns.
Game flow — pickup increments Score, maybe plays sound/particle and destroys the collectable; health decreases on damage, and player dies or respawns when health ≤ 0.
Step-by-step Implementation (Blueprint-first)
## Project & Input Setup
Create a Third Person Blueprint project (or use your existing ThirdPersonMap).

Open Project Settings → Input and ensure these mappings exist:

MoveForward (W / Up arrow)
MoveRight (A/D or Left/Right)
Turn / LookUp (mouse)
Jump (SpaceBar)
Run (Left Shift) — optional if you want sprint
2. Player Character Blueprint (BP_PlayerCharacter)
Duplicate the existing ThirdPersonCharacter (or create a new Character blueprint) and name it BP_PlayerCharacter.

## Variables to add (Expose where useful):

Score (Integer) — default 0
MaxHealth (Float) — e.g. 100.0
Health (Float) — default equal to MaxHealth
bIsRunning (Boolean) — if you want sprinting
Movement (in Event Graph):

Use Add Movement Input hooked to MoveForward and MoveRight axis mappings.
Use Turn and LookUp to rotate camera.
If using Run: on Run Pressed set Max Walk Speed on the Character Movement component (e.g. 1200) and reset on Released (600 default).
Health functions:

## Function: ApplyDamage(float DamageAmount)

Subtract DamageAmount from Health.
If Health <= 0 → call OnDeath event (disable input, play animation, respawn or show Game Over).
Update HUD (call event to update widget binding).
Function: AddHealth(float HealAmount)

Add to Health but clamp to MaxHealth.
Update HUD.
Score management:

## Function: AddScore(int Amount)

Score = Score + Amount → update HUD.
BP_Collectable → OnComponentBeginOverlap (Sphere)
Other Actor → Cast To BP_PlayerCharacter

## Branch (if cast success)

Call AddScore(ScoreValue) on Player Character
If GiveHealth > 0 Call AddHealth(GiveHealth)
Play Sound at Location
Spawn Emitter at Location
Destroy Actor
BP_PlayerCharacter → AddScore (Custom Event)
Input: Amount (int)
Score = Score + Amount
Call UpdateScoreDisplay on the HUD widget reference
(Optional) Play pickup sound, animate, or show floating text
BP_PlayerCharacter → ApplyDamage (Custom Event)
Input: Damage (float)

Health = Health - Damage

If Health <= 0

Call OnDeath (Disable Input; show Game Over)
Update HUD: Call UpdateHealthDisplay
## Output:

<img width="451" height="156" alt="image" src="https://github.com/user-attachments/assets/d44eb1d2-45d7-4618-babe-3b54c093f384" />
<img width="957" height="582" alt="image" src="https://github.com/user-attachments/assets/fbdc3026-3871-47de-9ef0-a970b6cd5657" />
<img width="921" height="458" alt="image" src="https://github.com/user-attachments/assets/7cca43a7-6367-42f9-adb0-7075e86f03b5" />
<img width="855" height="510" alt="image" src="https://github.com/user-attachments/assets/ae36377d-5aef-4011-8511-23828401ff34" />




## RESULT
The AI character successfully roams within the defined NavMesh area, choosing random destinations at intervals using the Behavior Tree logic.
