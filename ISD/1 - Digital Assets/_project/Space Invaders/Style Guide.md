---
tags:
  - S1
  - ISD
---
![[Theory#Style Guide]]



# Style Guide - Godot


# Specific Implementation

## Naming Conventions

In GDScript, the conventions for naming items are:

| Variables | snake_case |
| --- | --- |
| Constants | CONSTANT_CASE |
| Functions | snake_case |

It is also standard convention to include comment blocks at the top of the code to explain the code, but also to include function header comments to explain the purpose of each function.

### Example

```python
##	Detects when the player has collided with a Area2D, and checks the group. 
##	Code keeps the player within the boundaries of the game area by "bouncing" off the sides.
##	If the player colleided with the "left" group, the player is moved to the right
##	If the player collided with the "right" group, the player is moved to the left.
func _colliding(collided_area):
	if collided_area.is_in_group("left"):
		position.x=50
	if collided_area.is_in_group("right"):
		position.x=1230
```

## Code Commenting

Comments should be written in plain language to convey the meaning or purpose of the code to the viewer without them needing to read any of the specific code.

There are three areas to write code comments.

### Script Explanation or Summary

At the top of each script file, the **class docstring** should be included to explain the general purpose of the script.

```python
extends Node
## This singleton holds the global variables needed for the game to run.
## They are accessible throughout all scenes.
```

### Function Explanation or Summary

Each function written should have a comment explaining the purpose of the function.

```python
##	Detects when the player has collided with a Area2D, and checks the group. 
##	Code keeps the player within the boundaries of the game area by "bouncing" off the sides.
##	If the player colleided with the "left" group, the player is moved to the right
##	If the player collided with the "right" group, the player is moved to the left.
func _colliding(collided_area):
	if collided_area.is_in_group("left"):
		position.x=50
	if collided_area.is_in_group("right"):
		position.x=1230
```

### Inline Comments

Complex code sections should be explained in plain language.

```python
func _physics_process(delta):
	var collidedObject = move_and_collide(Vector2(0, -speed*delta))
	
	# If the bullet collides with an "Enemy" object, delete it 
	# and add to the player score.
	if (collidedObject):
		if "Enemy" in collidedObject.collider.name:
			collidedObject.get_collider().queue_free()
			GlobalVariables.scoringInformation["currentScore"] += 10
		queue_free()
```

# Practical Exercises

1. Update all existing code to match naming conventions & function header comments.
