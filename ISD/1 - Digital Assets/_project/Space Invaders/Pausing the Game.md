> [!info] Like with other areas of game development, there are many methods and approaches to implement a pause menu. The one shown in this tutorial will be a simple popup menu that stops the execution of the game until the resume button or key is pressed.

![[pauseMenuDemo.gif]]

# Global

To allow the code to work across multiple scenes and scripts, one way to store the name and location of the Pause Menu Node is in `global.gd`. The variable is defined here and accessed in the various scripts required.

Open `Global.gd`. Define a new global variable called `PauseMenu`

![[pauseMenuGlobal.png]]

```gdscript
@onready var PauseMenu # Stores the location of the Pause Menu in the scene.
```

Save the file.

# Main Game

Open the `MainGame.tscn`

In the scene create the following nodes in the hierarchy:


| Node Name    | Node Type   |
| ------------ | ----------- |
| CanvasLayer  | CanvasLayer |
| PauseMenu    | Control     |
| ResumeButton | Button      |
| Resume       | Node        |
The scene hierarchy should end up looking like this:

![[pauseMenuSceneNodes.png]]



Once the nodes have been created, select the four new nodes.

![[pauseMenuSelectNodes.png]]

With the nodes selected, change the `Mode` setting in the inspector to `When Paused`

![[pauseMenuWhenPaused.png]]

> [!faq] This allows these nodes to remain functional when the game is in its paused state.


# Input Map

Open the Project Settings (Project Menu -> Project Settings) and click on the Input Map tab.

Create a new action called `pause` and attach the escape key to it.

![[pauseMenuInputMap.png]]
Press the Close button.
# Scripting

## Resume Button Signal
Add a signal to the ResumeButton, linking the code to the script attached to the root node (`MainGame.gd`) as shown below.

![[pauseMenuResumeSignal.png]]


## `MainGame.gd`

Open `MainGame.gd` or whichever script is running the overall scene. 

In the `_ready()` function, set the PauseMenu variable defined in global to the location created in the scene nodes. Additionally, set the PauseMenu to be invisible so it's hidden when the game starts.
![[pauseMenuMenuSetAndInvisible.png]]

```gdscript
GlobalVariables.PauseMenu = $CanvasLayer/PauseMenu
GlobalVariables.PauseMenu.visible = false
```


## Resume button

Replace the contents of the new `_on_resume_pressed()` function with the following.

![[pauseMenuResumeScript.png]]

```gdscript
GlobalVariables.PauseMenu.hide()
get_tree().paused = false
```


## Input Handling

Still in `MainGame.gd` add a new function to handle the input from the user

![[pauseMenuCheckPauseInput.png]]

```gdscript
func _input(_event):
	if Input.is_action_just_pressed("pause"):
		if get_tree().paused == false:
			get_tree().paused = true
			GlobalVariables.PauseMenu.show()
			Input.mouse_mode = Input.MOUSE_MODE_VISIBLE
```

This code checks if the user has pressed the pause key (the escape key) and checks if the game is running normally. If both of those are true the game is paused and the pause menu is shown. It also reenables the mouse pointer so the user can click on the Resume button.

# Resume node

Right-click on the Resume node created earlier and attach a script, naming it `resume.gd`.

Replace the contents with the following code:

```gdscript
extends Node

func _input(_event):
	if Input.is_action_just_pressed("pause"):
		get_viewport().set_input_as_handled()
		if get_tree().paused:
			get_tree().paused = false
			GlobalVariables.PauseMenu.hide()
			Input.mouse_mode = Input.MOUSE_MODE_CAPTURED

```

This script unpauses the game, hides the Pause Menu from view and removes the mouse pointer. 

The other difference is that this code includes the line `get_viewport().set_input_as_handled()` which tells Godot to stop executing any other code that will be detect the pause button being pressed.

> [!faq] This code is the opposite of the code in the `_input` function in `MainGame.gd`.


# Why Resume code in two different places?

You may be confused why the code for pausing and unpausing the game is separated into two different places in the project (`MainGame.gd` and the Resume node). The reason is that when the game is paused, it halts **all execution of any code** in the game, unless a node is configured otherwise.

So, when the game is running, nothing is different from normal. But when the user presses the escape key, the main game is completed paused, **including accepting any more inputs from the user**. So if you were to try to detect any other escape key presses from within MainGame.gd, these would be ignored, not allowing you to resume the game.

Setting the four new Nodes setting `Mode` to **When Paused** allows these nodes to remain functional when the game is in its paused state, allowing the input to be detected, and thus allowing the game to detect and respond to the escape button to resume the game.


# Final issue

The final issue is that as it stands, the entire game is paused, **except for the countdown timer**. 

This is an easy fix. In `MainGame.gd` update the code which waits for the timer to complete by adding `, false` to the definition. This pauses the timer when the game is in it's paused state.

![[pauseMenuTimerUpdate.png]]

```gdscript
await get_tree().create_timer(1, false).timeout
```


![[commonBlocks#Commit & Push]]