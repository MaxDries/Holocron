---
tags:
  - S1
  - ISD
---
The Space Invaders game mechanic where the enemies move back and forth across the screen is a classic example of simple yet engaging gameplay. The player controls a cannon at the bottom of the screen and must shoot down waves of aliens that descend from the top. The aliens move in a grid formation, alternating between moving left and right, gradually moving down the screen as they do. The player must carefully aim and shoot to destroy the aliens before they reach the bottom of the screen and destroy the player's cannon. This back-and-forth movement creates a sense of urgency and tension, as the player must constantly adjust their aim and react quickly to the changing positions of the aliens.

There are a number of ways to implement this mechanic, the approach taken here is two-fold. 
1) The block of Enemies is moved as a whole.
2) If an enemy hits the edge of the screen, the whole block reverses direction.

# Block Movement

Right click on the `Enemies` Node and attach a script.

![[enemyMovementEnemiesAttachScript.png]]

Replace the contents with the code shown. This code simply moves all the nodes underneath the `Enemies` node (i.e. all the individual enemy nodes) on the `x` axis (horizontal) based on the value of `speed`.

![[enemyMovementEnemiesScript.png]]

```
extends Node2D

var speed = -200

func _ready():
	set_physics_process(true)
	
	
func _physics_process(delta):
	global_position.x += speed * delta

```

# Check Boundaries

Crucially, the enemy block needs to reverse direction when one of the enemies hit the edge of the screen/window. This can be done by modifying the value of `speed` set in `Enemies.gd` as defined above.

In the project, there are two areas defined on either side of the screen, one in the group `left` (on the left hand side of the window), and the other in a group called `right` (surprisingly, on the other side).

This section will make the `Enemies` block reverse direction when one enemy collides with one of these areas.

![[enemyMovementAreas.png]]

Open `Enemy.gd`.


Define a function called `_colliding` which will execute when the enemy instance collides with one of the areas. This code changes the value of `speed` by 

![[enemyMovementColliding.png]]

```gdscript
func _colliding(area):
	if area.is_in_group("right"):
		get_parent().global_position.y += 10
		get_parent().speed = -200
	if area.is_in_group("left"):
		get_parent().global_position.y += 10
		get_parent().speed = 200
```

Finally, update `_ready` to execute the `_colliding` function when an enemy collides with an area.

![[enemyMovementConnect.png]]

```gdscript
$Area2D.connect("area_entered", Callable(self, "_colliding"))
```

Save the File.

![[commonBlocks#Commit & Push]]


# Demonstration

Your enemies should now move similar to shown below.

![[enemyMovementDemo.gif]]