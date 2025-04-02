---
tags:
  - S1
  - ISD
---
The main menu has a number of buttons, but currently are not functional. This tutorial shows how to add simple functionality.

# Start Game Button

Open `res://Menu/Menu.tscn`. Find **Start Game Button** in the Scene Nodes List, 

![[mainMenuSceneNodes.png]]

Right click on it and choose **Attach Script**. Leave all the options as-is and click **Create**.

![[mainMenuAttachScript.png]]

Leave the file for the moment, and Save.
## Signals

> [!info] **Signals** in Godot are a messaging system that allows objects to communicate with each other by connecting functions to events (signals). When a signal is emitted, the connected function is called with the data that was passed to the signal. Signals can be used to trigger actions, pass data, and control the flow of execution.

With the **Start Game Button** node selected, click on **Nodes** on the top right.

![[mainMenuStartButtonNodes.png]]

Double click on entry `pressed`. This signal is called when the button is, surprisingly, pressed. This will trigger a particular function to run.

> [!info] The other signals can be used for different purposes as required. 

Change the **Receiver Method** to `_on_start_button_pressed`.

![[mainMenuConnectSignal.png]]

Click **Connect**. This will create a new function in `Start Game Button.gd` with the set function name.

In the `_on_start_button_pressed` function, replace `pass` with `	get_tree().change_scene_to_file("res://MainGame/MainGame.tscn")`

![[mainMenuStartButtonMethod.png]]

Save the file.

Run the game, and press the start button. This *should* take you to the Main Game scene.

![[commonBlocks#Commit & Push]]

# Quit Button

Repeat the same process as above, instead changing the method name to `_on_quit_button_pressed`. 

The code that goes in the function is : `get_tree().quit()`

![[mainMenuQuitButtonMethod.png]]

Save and test the button!

![[commonBlocks#Commit & Push]]
# Options Button

This button is a little bit more complex as there is no Options scene in the project. Before configuring the button, a scene needs to be created.

In the **FileSystem** window within Godot, find the **OtherScenes** folder. Right click on the folder, then **Create New**, then **Scene**

![[mainMenuOptionsCreateScene.png]]

Change the root type to **User Interface**, and name the scene **Options**. Click Ok.

![[mainMenuOptionsNameScene.png]]

# Options Scene

In this scene, you'll create a title across the top, a blank space in the middle (to be filled in later) and a button at the bottom which will return to the main menu.

![[mainMenuOptionsSketch.png]]


Right click on the **Options** root node, and choose **Add Child Node**.

![[mainMenuOptionsRightClick.png]]

Search for **VBoxContainer** and click Create.

![[mainMenuOptionsVbox.png]]

Change the name of the VBoxContainer to **Layout**.

![[mainMenuOptionsLayout.png]]

Create three children of the Layout node:
1) Label
2) HBoxContainer
3) Button

![[mainMenuOptionsComponents.png]]

Rename the children as **Title**, **Contents**, **ReturnButton**.

![[mainMenuOptionsComponentsReturn.png]]

## Screen UI


Select **Layout**. In the Editor, you'll find a tool menu, which includes the anchor options. Select the dropdown and choose the **Full Rect** option.


![[mainMenuOptionsAnchor.png]]

## Title Label

Select the Title node, and change the Text to "Options Menu". Also change the Horizontal Alignment option to **Center**.

![[mainMenuOptionsCentreTitle.png]]


## Return to Main Menu Button

Select the button, and change the Text to "Return to the Main Menu".

![[mainMenuOptionsButtonLabel.png]]

With the button still selected, choose the anchor menu again, this time setting the Vertical Alignment option to **Shrink End**, and select **Align with Expand**. This will move the button to the bottom of the screen.

![[mainMenuOptionsButtonAnchor.png]]

### Button Functionality

Taking the same approach as done when adding functionality to the [[#Start Game Button|Main Menu's Start Button and Quit buttons]], add a script to the button. Attach a `pressed` signal to the button, with the method name `_on_return_pressed`.

Replace `pass` with `get_tree().change_scene_to_file("res://Menu/Menu.tscn")`.

![[mainMenuOptionsReturnButtonCode.png]]

Save the file.

## Contents Node

Select the Contents node and set the anchor to **Fill**.

![[mainMenuOptionsContentsAnchor.png]]

Save the Scene.

## Update Main Menu

Now that the scene has been created, add functionality to the Options button to link to the new scene.

Connect the pressed signal to the script attached to the Options button. **You may find you need to select the Options button in the list**

![[mainMenuOptionsSignal.png]]

Set the function to open the Options scene.

![[mainMenuOptionsButtonCode.png]]

Save!

# Test the updates

Run the game and confirm the buttons are functional.

![[mainMenuButtonsWorking.gif]]


![[commonBlocks#Commit & Push]]

