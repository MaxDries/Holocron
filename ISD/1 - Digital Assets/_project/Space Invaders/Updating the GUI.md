---
tags:
  - S1
  - ISD
---

![[Theory#Algorithm Design - Sequence]]

# Specific Implementation

When programming in GDScript, you will be writing many blocks of code that are in a sequence.

> In this example here, you will see that the function `_ready()` (more on this later) contains a sequence of instructions. 
> 
> ![[sequenceExampleVariables.png|Screen Shot 2022-01-17 at 9.38.14 pm.png]]
> 
> Line 11 declares a new variable (with the `var` keyword), called name which is assigned the data “Player One”. Using type inference, this variable is defined as a string.
> 
> Line 12 is similar to line 11, however this variable is named welcome and is assigned the string “Hello”
> 
> Line 13 defines a new variable called message, and the data that is assigned is a concatenation of three strings - `Hello`, a space, and `Player One`.
> 
> Line 14 is blank and is therefore ignored when it is executed.
> 
> Line 15 outputs the contents of the message variable to the console within Godot.


 > [!info] When programming in GDScript, **indentation** is critical. Code that needs to be part of a loop, decision or function needs to be indented at the same level (tabs) to be considered coded correctly.


This code is a sequence because it is run sequentially - Line 11 is run, then Line 12, then 13 etc. 

It’s also important to note that each line of code does not have any understanding of what is before or after it - so when the print command is executed on Line 15, it does not know what is in message until it is run.

It’s also important to note that as a developer, you can change the lines of the code prior to it and the other lines of code will not be effected, as long as you don’t change the variables and/or data types involved. So, you could modify the code to accept the players name from a dialog box on screen and the remainder of the code will still run.

# Practical Exercises

Open the Space Invaders project, and open the `MainGame/MainGame.gd` script file. 


> [!info] This script is attached to the MainGame scene and runs when that scene is loaded when the game is executed. In this game, this scene is the first scene run.


![[sequenceMainGameScript.png|Screen Shot 2022-01-17 at 9.54.53 pm.png]]

Under the extends Control line, remove the commented lines about declaring variables - this is default code when a new script is created. 

In this section of code, you will be developing the code to link a onscreen element to code. In this case, you’ll be initialising the countdown timer, so your game has a time limit.

Create two new variables. The first is the amount of time available to the game - call this `countdownMax`. The second is the current time, which is reduced by 1 every second - call this `currentTimer`.

![[sequenceVariableDeclare.png]]


```gdscript
@export var countdown_max: int = 10
var current_timer
```

You’ll notice that countdownMax has a few extra commands. The keyword export allows the variable to be accessed outside of the script, specifically through the Godot IDE, and int is informing the IDE that it is an integer. 

When you have the MainGame node selected in the hierarchy, you’ll be able to access the Script Variables in the inspector.

> [!info] **Why would you want this?** 
This allows the same script to be used in different ways. For instance, you could use this same script on different levels with different starting times, but the remainder of the code logic is the same.

Now that the variables have been defined, it’s time to move to the `_ready` function.

![[sequenceVariableInInspector.png]]

In the `_ready()` function - under `func _ready():` - assign the value of `countdownMax` to the `currentTimer`. This is done so later in the code, you will reduce `currentTimer` by 1, but leave the original value alone.

The next line of code is more complex. First, consider the whole line:

`$HUD/Countdown.text = str(currentTimer)`

Starting on the right-hand side, this gets the value that is stored in currentTimer, and passes it as an argument to the `str()` function. The `str()` function converts an integer to a string. 

The result of that function (converting the int to a string) then gets assigned to `$HUD/Countdown.text` which is GDScripts syntax for “Look in the hierarchy for something called `HUD` which is a child of the node that this script is attached to. Underneath that is something called `Countdown`, and look for the text component in that.” 

Or in more programming terms - access the text property of the Countdown node, which is a child of HUD and set the value.

![[sequenceCountdownSetValueGUI.png|Screen Shot 2022-01-17 at 10.18.11 pm.png]]

```
current_timer = countdown_max
$HUD/Countdown.text = str(current_timer)
```

Set the starting time in the IDE to a value (say 50).

Make sure you select the Node in the hierarchy!

![[sequenceSetMaxTime.png|Screen Shot 2022-01-17 at 10.20.43 pm.png]]

## Run the Game

Press the Run button in the top right to run the game. 

![[sequenceRunScene.png]]

You should notice that it opens into a very uninteresting game which no movement or interaction, however the timer in the top left hand corner of the window should show the value that you set as the starting timer for the game!

Well done!

![[sequenceGUIUpdated.png]]

## What’s next??

Over the next few weeks you’ll develop this game into much more - the timer will count down, the enemies will move (you can also change the images if you wish), and you will be able to move the player and shoot!

# Review

1. Write the sequence of steps to make and eat toast.
2. Write the sequence of steps to 
	1. Read in individual lines of a text file (use the one shown)
	2. Calculate the GST
	3. Output the GST component to the user.
		
		![[loopReviewValues.png|Screen Shot 2022-01-27 at 9.44.53 pm.png]]
		

