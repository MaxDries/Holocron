---
author: Ryan Cather
date: 2024-06-09
priority: 1
order: 2
tags:
  - ISD
  - yr11
---

As this game is a First-Person Shooter, there is no need to create a mesh for the player object, as in game, the player will never see it.

Therefore, the player object can be initially created with a simple collider and camera.

There are two main steps to creating the player:

1. Making the player scene, creating nodes etc, and 
2. Coding the functionality.

# The Player Scene

Create a new Scene. 

Create the root node as `CharacterBody3D`. To do this, click on Other Node and search for `CharacterBody3D` and choose Create.

Save the scene as `Player.tscn`.

![[fpcSaveScene.png|Save Scene]]

Rename the root node to `Player`.

![[fpcRenameRoot.png|Player]]

Add a child node - `CollisionShape3D`.

This will represent the boundaries of the player node in game. It will be used as the players ‘hit box’ (officially referred to as the **collider**) to detect collisions - walls, floors, projectiles etc.

![[fpcCollider.png|Collider]]

With the `CollisionShape3D` selected, create a `new CapsuleShape3D`.

![[fpcCollisionShape.png|Collision Shape]]

Resize the dimensions of the capsule, but clicking drop down box next to Capsule Shape and choose Edit.

![[fpcEditCapsuleShape.png|Edit Capsule Shape]]

Set the Radius and height to values that suit your game.

> [!note] You may find these values need to be modified later to better suit your game.


![[fpcRadiusHeight.png|Set Radius and Height]]

## Camera

The player has been created, however the player cannot ‘see’ as a camera has not been added. The camera is what the user sees in game. In an FPS, the mouse is attached to the player object and therefore the camera. When the mouse moves, the camera moves to match.

Add a `Camera3D` child node to the Player node.

![[fpcCamera3d.png|Add Camera3D]]

Set this camera to be the ‘main’ camera by setting the Current attribute to True.

![[fpcCameraCurrent.png|Set Camera to current]]

Move the camera up the ‘body’ to appear as if the camera ‘sees’ through the players eyes.

> [!note] Exact value is not critical, but it has to suit your capsule size set earlier.


Only change the `Y` value. This is the vertical value. The coordinate system will be discussed at a later time.

![[fpcCameraYValue.png|Set the V value]]

## Key Actions

Before any scripting can be done, some keyboard and mouse inputs need to be mapped to actions (to be coded later).

Open `Project` → `Project Settings` → `Input Map`. Click the toggle to `Show Built-In Actions`.

![[InputMap-BuiltInActions.png|Show built-in actions]]

Find the input’s you wish to set, and click the edit button to set a new button.

You will need to edit/set the following values:

| Action     | Key |
|------------|-----|
| `ui_left`  | A   |
| `ui_right` | D   |
| `ui_up`    | W   |
| `ui_down`  | S   |


![[InputMap-correct.gif|inputmap-correct.gif]]

Create new inputs for the following actions:

| Action         | Key   |
|----------------|-------|
| `player_shoot` | LMB   |
| `player_jump`  | SPACE |


![[InputMap-new.gif|inputmap-new.gif]]


>️ [!note] Pay close attention to the spelling and capitalisation. These will be linked to in the script later.


## Mouse Look

Open `Player.tscn`. Right-click on the root node and attach a script. Leave the settings as they are, and click Create.

![[fpcPlayerAddScript.png|Attach Script]]

First, the jumping. Change the value for the jump input map to `player_jump` from ui_accept.

![[fpcScriptJump.png|Change Jump input]]

Add a new variable to dampen the speed at which the mouse moves.

The value can be changed at a later point, however start with 0.002.

![[fpcScriptMouseSensitivity.png|Mouse sensitivity]]

Add a new `_ready()` function which will hide the mouse pointer and allow the camera to follow where the mouse is moved.

![[fpcScriptMouseCapture.png]]
```gdscript
func _ready():
    Input.mouse_mode = Input.MOUSE_MODE_CAPTURED
```



Add a new `_input()` function to get the movement of the mouse, and get the rotation of the player camera to match.

![[fpcScriptAddInputFunc.png]]

```gdscript
func _input(event):
    if event is InputEventMouseMotion and Input.mouse_mode == Input.MOUSE_MODE_CAPTURED:
        rotate_y(-event.relative.x * mouse_sensitivity)
        $Camera3D.rotate_x(-event.relative.y * mouse_sensitivity)
        $Camera3D.rotation.x = clampf($Camera3D.rotation.x, -deg_to_rad(70), deg_to_rad(70))
```


# Add the Player to the Scene

Open the Scene/s that you wish to add the player to.

Drag the `player.tscn` file into the hierarchy. Initially this will be `level_one.tscn`.

![[fpcAddPlayer.png|Add Player]]

# Test the game.

Run the game, and click the start game button on the main menu.

![[fpcPlayerInGame.gif|player.gif]]

![[commonBlocks#Commit & Push]]

# Player Run Functionality

To implement a simple run functionality, `Player.gd` can be updated to modify `SPEED` based on whether the shift key is held down or not.

First, add a new Input into the Project Settings → Input Map page.

Name it `player_run`.

Then add the shift key to that map.

![[fpcInputMapShift.png|Untitled]]

Change the `SPEED` variable to **not** be a constant. This will allow you to change the value in code.

![[fpcSpeedVariable.png|Untitled]]

Open `Player.gd` and find the `_physics_process(delta)` function. Add the following if statement to set `SPEED` to change based on the button press.

> The exact speed values can be modified based on the requirements of the game.

![[fpcSpeedShiftCheck.png|Check if shift is pressed]]

```gdscript
if Input.is_action_pressed("player_run"):
	SPEED = 10.0
else:
	SPEED = 5.0
```


![[commonBlocks#Commit & Push]]