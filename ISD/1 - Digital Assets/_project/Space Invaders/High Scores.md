---
tags:
  - S1
  - ISD
---
> [!important] This tutorial gives you the evidence to complete VET competency `ICTPRG302 Apply Introductory Programming Techniques`, performance criteria 4.1 & 4.3

> [!important] Prerequisites: [[ISD/1 - Digital Assets/_project/Space Invaders/Data Structures|Data Structures]]
# Saving the Score

In this tutorial, you'll be storing the players score as persistent data, so subsequent playthroughs can keep a track of the high score.
## Persistent Data

> [!info] The data stored on HDD/SSD, is referred as `persistent data`.

Before implementing any form of persistent data, the exact data that needs to be save must be determined. The points where the data is loaded from and saved to the local drive must be decided.

Your application must also be prepared for attempting to load the data, however it doesn’t exist at that time - such as when the application is run for the first time.

Godot has an inbuilt system to save data and nodes to the local drive, however this tutorial will focus on only saving and loading the previous high scores.

Before implementing persistent data, there are a few topics to understand - where the user data is stored.

## Godot User Data

Most file access performed in Godot begins with `res://` (e.g. `res://Menu/Menu.tscn`), which refers to the project root folder (the folder containing `project.godot`). A file path starting with `res://` looks for the file relative to that root folder.

User data uses the `user://` prefix which points to a different folder which is not in the project folder.

> [!important] The `user://` prefix points to a different directory on the user's device. On mobile and consoles, this path is unique to the project. On desktop, the engine stores user files in:
> `~/.local/share/godot/app_userdata/[project_name]` on Linux, 
> `~/Library/Application Support/Godot/app_userdata/[project_name]` on macOS (since Catalina) and
> `%APPDATA%\Godot\app_userdata\[project_name]` on Windows.
> 
> [https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html)




# Update `Global.gd`

Open `Global.gd`. The `scoring_information` dictionary containing the current score and high score keys will be already defined during the [[ISD/1 - Digital Assets/_project/Space Invaders/Data Structures|Data Structures]] stage. 

Create a new variable to link to the save file.

![[persistentDataSaveFile.png]]

```gdscript
var save_file = "user://save.txt"
```

Save the file.
## Win Scene

*Open* or *create* a `win.tscn` in your project and attach a script.

![[persistentDataWinNewScript.png]]

In the new script create a `save_data()` function, calling it from `_ready()`.

![[persistentDataScriptFunction.png]]

Inside `save_data()`, the connection to the file is created, and opened to write data to it. 

After the connection is made, you simply write the value of the variable to the file using `store_var` which uses the Godot built in serialisation process.

> [!info]- `set_value()`
> When saving individual variables to the file, you use the `set_value` function. The function takes three arguments.
> **First argument** - The *section*. This is a way to organise the save file. For instance, all the Player variables would be stored in the Player section.
> **Second argument** - The *key*. This is equivalent to the variable name. It's organised like a key-value-pair or dictionary where keys have a particular value.
> **Third argument** - The *value*. This is the actual data that you want to save.
> So the code below would create a "Player" section, which contained a "score" key, with a specific value attached.

![[persistentDataFuncSaveData.png]]

```gdscript
func save_data():
	var file = ConfigFile.new()
	file.set_value("Player", "high_score", GlobalVariables.scoring_information["highScore"])
	var error := file.save (GlobalVariables.save_file)
	
	if error:
		print("!!Data Not Saved!!")
```

## Test the saving process

Run through the game, killing all the enemies. 

After the Win scene has been loaded, you should find `save.dat` in the user directory (see above). You'll notice that the structure of the file matches the `set_value` function with `"Player"` (the section) and `"score"` (the key).

![[ISD/1 - Digital Assets/_project/Space Invaders/_images/persistentDataFileContents.png]]

![[commonBlocks#Commit & Push]]
# Loading Data

Now that the data is stored on the local drive, it can be loaded when the game starts.

Open the main menu (`Menu.tscn`). Attach a script if there's not already one attached.

Create a new function - `load_data()`. This function will need to first check if the `save.dat` file exists, if so, then open it to read the contents.

![[persistentDataFuncLoadData.png]]



```gdscript
func load_data():
	var config_file := ConfigFile.new()
	var error := config_file.load(GlobalVariables.save_file)
	if error:
		print("An error happened while loading data: ", error)
		return
	
	# Load specific data
	GlobalVariables.scoring_information["highScore"] = config_file.get_value("Player", "high_score", 0)
	print(GlobalVariables.scoring_information["highScore"])
```

Repeat the process with another piece of data, such as the players name, or the value of current timer.

# What's Next?

The next stage is to compare the current players score against the high score (with an `if`). If the score is higher, override `Global.high_score` with the new value.

You will also want to display the high score to the user, possibly in the Win or Lose scenes.

