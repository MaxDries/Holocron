---
tags:
  - S1
  - ISD
---


> [!info] Raycasts are an invisible line between two points in game. Using Raycasts you can detect collisions (or potential collisions). They have a number of uses such as calculating the destination of a bullet, without creating (instantiating) a bullet in game, or if an object can ‘see’ another object (if there’s a wall in the way).

# Add Raycast

In the project, open the `Enemy.tscn` file to edit the enemy object. 

> [!important] This will update **all** enemy instances in the game, as the original scene is being modified.


Add a child node to the Enemy. Search for RayCast2D in the list and choose Create.

![[ISD/1 - Digital Assets/_project/Space Invaders/_images/raycastAddChild.png]]


With the new RayCast2D node selected, change the Inspector values `Target Position Y` to 200.

![[raycastSetTarget.png]]


Attach a script to the RayCast2D node.

  
![[raycastScriptAttach.png]]


  
![[raycastScriptName.png]]


Replace the contents with the following code. This code will check if the ray is colliding with something, and if it is, then it sets the `can_shoot` (to be defined below) variable to true, otherwise it is false.

![[raycastScript.png]]

```gdscript
extends RayCast2D

# Called when the node enters the scene tree for the first time.

func _ready():
	set_process(true)

func _process(delta):
	if self.is_colliding():
		get_parent().can_shoot = false
	else:
		get_parent().can_shoot = true
```

Save the script.

# Enemy Script Update


Open `Enemy.gd` and create a new variable, declaring it at the top of the script.

```gdscript
var can_shoot = false
```

  
![[raycastEnemyVariable.png]]


**Update** the `fire_bullet()` function to first check if `can_shoot` is true. If it is true, then the normal ‘shooting’ code will execute.
  
![[raycastCheckCanShoot.png]]

Save the file.

![[commonBlocks#Commit & Push]]


# Confirmation

Check to ensure that it works correctly. Is it?

If not, what is occurring and:
- What could be the cause, and
- What could be the solution?