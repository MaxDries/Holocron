---
tags:
  - S1
  - ISD
---
> [!important] Prerequisite: [[Updating the GUI]]

![[Theory#Algorithm Design - Loops]]

# GDScript Implementation

The GDScript language has  two loop structures identified on the General Learning Resource - `while` and `for` but not `do..while` . 

Here you can see the syntax and an example of each.

## While Loop

```python
while [expression]:
	//statement(s)
```

### Example

```python
while countdownTimer < 50:
	countdownTimer = countdownTimer - 1
```

## For Loop

for loops in GDScript are a little different than the syntax shown in General Resources. The syntax mirrors the for in loop in Python. Technically, they are called `for-in` loops and they hide much of the syntax that is required with a “traditional” for loop. 

They are more aimed at iterating over arrays or dictionaries and similar structures.

These types of for loops can be used in different ways depending on the purpose.

### Examples

A simple example of the for-in loop is:

```python
for i in range(3):
	print(i)
```

This example shows a loop which will iterate three times. The variable `i` can be accessed and used in the statement blocks, as shown where it will print out:

`0`

`1`

`2`

---

In the example below, the loop executes (iterates) three times, with the value stored in x changing for each iteration. The first time it iterates, x is 5, then it is 7 and finally 11.

```python
for x in [5, 7, 11]:
	statement # Loop iterates 3 times with 'x' as 5, then 7 and finally 11.
```

![https://www.youtube.com/watch?v=GS-lAZBNm6k](https://www.youtube.com/watch?v=GS-lAZBNm6k)

# Practical Exercises

Previously, a countdown timer was configured, but without loops, it would be difficult to fully implement. Now’s the time.

Open the ongoing project and open the `MainGame.gd` script. This script will be expanded to reduce the `current_timer` by 1 each second. 

When the timer reaches 0, a message will be printed to the console. This can be modified later to quit the game, or display a dialog box or similar, however at this stage, this will be fine.

![[loopsCountdownStart.png]]

In `_ready()` create a while loop which will run as long as `current_timer` is above 0.

> [!info] In this case, the while loop is the most appropriate choice, over a for loop, as current_timer needs to be modified within the loop iteration. In a for loop, the value is readonly.


![[loopsCurrentTimerStart.png]]


```gdscript
while current_timer > 0:
```

## Loop Logic

Update the while loop to execute the following logic:

- Pause for 1 second
- Reduce the timer by 1
- Print the timer value to the console (for debugging)

## Loop Code

The code shown achieves this, but let’s go into the details.

`yield()` is the GDScript function to pause the execution of the current code block, *but still allow other functions to run*. This is important as you wouldn’t want the game to freeze for a simple counter. This is referred to as a **coroutine** function.

`yield()` pauses for 1.0 seconds (`get_tree().create_timer(1.0)`) and when the timer expires, the “`timeout`” signal is issued, indicating to Godot that the execution can continue to the next line of code.

`current_timer = current_timer - 1` subtracts 1 from current_timer and replaces the contents with the new value.

`print(current_timer)` outputs the value of `current_timer` to the console.

![[loopCountDownloop.png]]

```python
while current_timer > 0:
	await get_tree().create_timer(1).timeout
	current_timer -= 1
	print(current_timer)
```

If you run the project now, the game itself wouldn’t change, however you would see this in the console.

![[loopCountdownDebug.gif]]

## Update the Game User Interface

Luckily, updating the UI is straightforward.

Update the `while` loop to change the text property of the Countdown node as has been done previously.

![[loopUpdateGUI.png]]

```python
$HUD/Countdown.text = str(current_timer)
```

## Timer 

When `current_timer` reaches 0, the loop will complete and not iterate again.

Add a `print()` command to output “Game Over” to the console.

![[loopGameOver.png]]

## Run the Game

Run the project, and make sure that the timer counts down, all the way to 0. It should then output “Game Over” to the console.

![[loopCountdownGUI.gif]]

# Review

1. What is the difference between the available types of loops? Why choose one of the other/s?
2. How many times can loops iterate?
3. As an update to last weeks exercise write the steps in plain language, **using loops**, to 
	1. Read in individual lines of a text file (use the one shown)
	2. Calculate the GST
	3. Output the GST component to the user.
		
		![[loopReviewValues.png]]
		



