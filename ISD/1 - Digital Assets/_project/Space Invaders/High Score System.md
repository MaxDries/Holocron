---
tags:
  - S1
  - ISD
---
![[Theory#Data Structures]]

# GDScript Implementation

## Arrays

Arrays in GDScript operate very similar to other languages and the syntax is similar as well, aligning most closely with Python.

In GDScript, unlike some other languages, arrays can have a mix of data types for the elements. It’s perfectly valid to have an array such as `[int, string, bool]`

> [!important] In GDScript, array indexes start at 0

### Declaring Arrays

To declare and empty array, you declare it similar to other variables:

```python
var arr = [] # This creates an empty array.
```

To declare an array with initial values, you can declare it similar to this:

```python
var array = [1, 2, 3]
```

### Accessing Array Elements

In code, you can access (read or write) to specific indexes by specifying the index inside the square brackets. 

> [!tip] For example:
>
![[dataExampleArray.png]]

## Key Value Pair

Dictionaries use the curly brackets - `{ }` - to indicate the start and the end of the structure when declaring the initial values.

The item on the left of the equals sign is the `key`, and the item on the right is the `value`. 


> [!info] In GDScript, Key Value Pairs are referred to as *Dictionaries*.


### Dictionary Example

In this example, the dictionary called dict, contains three entries. The three keys are - `name`, `score`, and `time`. The values are associated with the indicated key.

`print(dict["name"])` will output “User” to the console.

![[dataExampleDictionary.png|Screen Shot 2022-01-20 at 11.40.45 pm.png]]

# Practical Exercises

## High Score

Open `Global.gd` in the root of your project. 

There is some code in there already, which you can use at a later stage, however in this stage, you’re going to implement a dictionary.

![[dataGlobalGD.png]]


> [!info] This code file is different from other code in the project. So far the code you’ve written has been attached to a particular node. This `Global.gd` is a script which loads when the project loads and is referred to as a `Singleton`. Singletons are useful for when you need to store information across scenes and aren’t loaded and unloaded from memory when nodes are loaded and unloaded.
Information Singletons can be found on the link here.
[Singletons](https://docs.godotengine.org/en/stable/tutorials/scripting/singletons_autoload.html)

### Global.gd

Declare a dictionary called `scoringInformation`. 

This variable will store:

- currentScore
- currentScorePlayersName
- highScore
- highScorePlayersName

![[dataScoringInformation.png|Screen Shot 2022-02-26 at 1.32.37 pm.png]]

```python
var scoring_information = {

}
```

Create the four entries inside the dictionary. 

![[dataScoringInformationComplete.png|Screen Shot 2022-02-26 at 1.55.49 pm.png]]


```python
var scoringInformation = {
	"currentScore": 0,
	"currentPlayer": "User",
	"highScore": 0,
	"highScorePlayersName" : "Winner"
}
```

Now to access and update the dictionary from another script!

### Bullet.gd

Open the `\Bullet\Bullet.gd` script.

This script is attached to each bullet that gets instantiated in the game - i.e. each bullet that gets created when the user presses the fire button.

Currently, this script simply moves the bullet object up through the `move_and_collide()` function.

![[dataBulletMove.png]]

If the bullet collides with an object (say an enemy), that object contains the details on the collision, including the object that it collided against. The code needs to check to see if that object was an Enemy object, and if so, add to the current high score.

First, the code needs to check if the bullet has collided with an object in `_physics_process()` . This is done by checking if collidedObject has data and is not null (empty). Add code to check if the bullet has collided with an object and just print out the name of said object.

![[dataCollidedObjectName.png]]

```python
if (collided_object):
	print(collided_object.get_collider().name)
```

> [!info] At this stage, you can run the game, shoot and see the console for the outputted object names. As each enemy object start with the word Enemy, this can be used to confirm if the bullet has collided with an enemy object.
![[dataObjectCollisions.png]]


Add code to check if the collided object is an enemy and if so, delete it from the game.

Run the game, and you should find that the enemy objects get deleted when the bullets collide with them.

One problem you’ll notice is that the bullet *doesn’t* get deleted. This will need to be fixed.

![[dataDetectKillEnemy.png]]

```python
if "Enemy" in collided_object.get_collider().name:
	collided_object.get_collider().queue_free()
```

Delete the bullet from the game when it collides with anything.

Notice how the `queue_free()` command runs after any collided object is detected, and also whether or not this collided object is an enemy or not. This could be useful further in the development process if barriers are implemented in the game, or for other purposes.


> [!info] `queue_free()` deletes the node from the game at runtime.



![[dataDeleteBullet.png|Screen Shot 2022-02-26 at 2.19.36 pm.png]]

```python
queue_free()
```

Finally, update the if block of code to not only delete the enemy but increase the Current Score by 10.

This access the `Global.gd` script through the object name `GlobalVariables`. This is configured in the Project Settings under AutoLoad.

The `scoringInformation` dictionary is then used to access the `currentScore` entry and adds 10 to the value.

> [!info]  `+= 10` adds 10 to whatever the integer value is at that time.


![[dataDictionaryUpdate.png|Screen Shot 2022-02-26 at 2.22.17 pm.png]]


```python
GlobalVariables.scoring_information["currentScore"] +=10
```

The game now tracks the score, it’s now time to update the user interface with the score so the user can see it.

### Display the Current Score In Game

Open `MainGame.gd` and find the code to update the current timer on screen. 

![[dataGUIUpdateTimer.png|Screen Shot 2022-02-26 at 2.32.49 pm.png]]

After updating the display with the current timer, the current score can also be updated in the same fashion. However, instead of using `currentTimer`, the `scoringInformation` dictionary will be used from `GlobalVariables`.

Update the CurrentScore label with the `currentScore`.

![[dataGUIUpdateScore.png|Screen Shot 2022-02-26 at 2.36.09 pm.png]]


```python
	$HUD/CurrentScore.text = str(GlobalVariables.scoring_information["currentScore"])
```

Run the game and see how the score updates!

![[dataGUIUpdating.gif]]

### Room for Improvement?

The game runs, enemies get destroyed and the score gets updated, however is that the best it can be? You may notice that the score only updates on screen every second with the timer. Is there some other way the score could be updated in a more responsive manner? For instance update when the enemy gets destroyed? Or ‘continually’ update?

> [!info]- Answer
>Instead of updating the score after a 1 second delay in the `while` loop, in [MainGame.gd](http://MainGame.gd) create a `_process` function which updates the CurrentScore instead.
> 
> Add the following code to [MainGame.gd](http://MainGame.gd) and remove the same line of code from `_ready()`
> 
> ```python
> func _process(delta):
> 	$HUD/CurrentScore.text = str(GlobalVariables.scoringInformation["currentScore"])
>```
> ![[dataAnswer.png]]


Change the name of the first to CurrentScoreLabel and the next CurrentScore.

Change the text attribute of CurrentScore to “0”.

## Review

1. Could the Scoring system implemented above through a dictionary have been implemented using an Array? If so, how? What changes would need to be made?
2. Research other data structures available in Arduino and other languages, such as linked lists. For each:
3. What are multidimensional dimensional arrays? Describe them and show code examples for what they are and what they can do. What situations would you use a multidimensional array over other data structures?



