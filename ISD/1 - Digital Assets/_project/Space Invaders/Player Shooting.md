---
tags:
  - S1
  - ISD
---
![[Theory#Algorithm Design - Modularisation]]


# GDScript Implementation

Functions in GDScript work similarly to functions in other languages - they are designed to organise and simplify your code, and allow you to reuse blocks of code.

`print()` is a built in function that allows you to output data to the console for debugging purposes - the player of your game would never see the output.

`_physics_process()` is a function which is called when physics (gravity, movement etc) needs to be applied to an object in the game.

Like other language, GDScript functions can accept parameters - such as `delta` in the example, and can also optionally return data with the `return` keyword.


> [!info] The keyword for writing functions in GDScript is `func`.
> ![[modularisationExampleFunction.png|Example function from Player.gd]]
> Example function from Player.gd

## Parameters

GDScript functions can have parameters or not.

### Example - Function Without Parameters

```gdscript
func my_function():
	pass
```

### Example - Function With Parameters

```gdscript
func my_function(a, b):
	pass
```

If the data type of the parameters are not specified, the function can accept any variable, however this may not be ideal in all cases. It is possible to specify the data type of parameters in the function header. It is also possible to specify default values in parameters.

### Example - Parameters with specified data types

```gdscript
func my_function(a: int, b: String):
	pass
```

### Example - Parameters with default values

```gdscript
func my_function(int_arg := 42, String_arg := "string"):
	pass
```

## Return Data

Like other languages, GDScript languages can return data or not, depending on the needs and structure of the code.


> [!info] Functions without a return keyword will return null by default.


### Return Example

```gdscript
func my_function(a, b):
	print(a)
	print(b)
	return a + b  # Return is optional; without it 'null' is returned.
```

### Return Example Specifying Data Type

Functions can also specify the return data type. This may be required if writing large code libraries, or for more robust code avoiding type errors. To specify the return data type, use the `->` symbol. 

```gdscript
func my_int_function() -> int:
	return 0
```

# Practical Exercises

To demonstrate function implementation in GDScript, player shooting will be coded, as well as limiting the player to within the game area.

## Player Shooting

The logic for this section of the code is:

- Check if the player presses the space bar.
- If so, create a bullet object above the player object
- Add movement to the bullet to move vertically

Open `Player.gd` and create a new global variable to store the image of the bullet that will be fired.

Using the `preload` keyword means that the bullet scene (`Bullet.tscn`) is loaded into memory before the game scene loads. In this case it may not be an issue, however if the object being loaded was resource intensive, using preload may avoid the game lagging when loading objects.

![[modularisationSpeedAndBulletSource.png|Screen Shot 2022-01-19 at 11.36.41 pm.png]]

```gdscript
var movement_speed = 200
var bullet_source = preload ("res://Bullet/Bullet.tscn")
```


Update `_ready()` to run `_process()` every frame refresh.

> [!info] You will need to replace the existing code - `pass` from within the function.

![[modularisationSetProcess.png|Screen Shot 2022-01-19 at 11.34.46 pm.png]]


```gdscript
set_process(true)
```

The next step is to complete `_process()`. This function will check if the player presses the space key, and then create a bullet instance in the game.


![[modularisationProcessFunction.png]]

```gdscript
func _process(delta):
	if Input.is_action_just_pressed("fire"):
		var bullet_instance = bullet_source.instantiate()
		bullet_instance.position = Vector2(position.x, position.y-20)
		get_tree().get_root().add_child(bullet_instance)
```

Here’s the explanation:

**Line 11**: this if statement checks to see if the player pressed the fire button (the space bar in this case).

> [!info] `if` statements are discussed in more detail in [[Player Movement]]

**Line 12**: Creates, in memory, a new **instance** of the bullet scene.

> [!info] An instance is an individual implementation of a specific object. Therefore you can have multiple instances of a bullet, based on the original object.

**Line 13**: Sets the position of the bullet instance to the `x` and `y` coordinates of the player object, with reducing the `y` coordinate value by 20 pixels. This places the bullet just above the player object. You may change this value to suit the game.

**Line 14**: Creates the bullet instance in the game scene. Up until this line, the bullet has only been instantiated in memory. This line puts it into the scene as a child of the player object.


## Add Force

The code, up until now, only creates the bullet instances. If you ran the game now, you’ll see that the bullets get created, but don’t move. In order to achieve movement, each individual bullet instance will need to be effected. This can be done through the bullet object itself.

Open `Bullet\Bullet.gd` and update the script to apply movement to the bullet.

The code in this script is very similar to some of the code in `Player.gd`. 

The values given on Line 10, mean that the bullet won’t change on the `x` axis (left and right), but only move upwards on the `y` axis.

![[modularisationAddForce.png]]

```gdscript
extends KinematicBody2D

var speed = 500

# Called when the node enters the scene tree for the first time.
func _ready():
	set_physics_process(true)

func _physics_process(delta):
	var collidedObject = move_and_collide(Vector2(0, -speed*delta))
```

## Run the Game

When you run the game at this stage, you should see the player can move, and bullets can be fired.

You may notice a few bugs or issues with this functionality at this stage. What issues can you see, and how would you go about fixing them?

![[modularisationRunningExample.gif]]

