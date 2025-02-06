---
tags:
  - S1
  - ISD
---
There are two main uses of audio in a game:

- Background Music
- Event-based Feedback

> [!info] The starter project includes two .WAV files that can be used - `backgroundMusic.wav` and `bulletFire.wav`. More audio files can be added and used if you wish.

# Video

![https://www.youtube.com/watch?v=wKs7a6RBfaw&ab_channel=RyanCather](https://www.youtube.com/watch?v=wKs7a6RBfaw&ab_channel=RyanCather)

# Background Music

Background music is music that runs continually in the game to add ambience to the players experience. 

Whatever music file you use, it needs to be looped. Select the file in the FileSystem (in this case `backgroundMusic.wav`), change to the Import tab at the top, and set the Loop Mode to **Forward**. Finally, click `Reimport`.

![[audioLoopImport.png]]

This sets the file to be looped anytime that file is used within the project.

Switch back to the Scene tab at the top. Create a child node of the root node (`SpaceInvaders`) and choose `AudioStreamPlayer2D`.

![[audioStreamChild.png]]


Drag the audio file from FileSystem to the **Stream** setting in the inspector and tick **Autoplay**.

![[audioSetStream.png]]

Save the file.

![[commonBlocks#Commit & Push]]


# Event-Based Feedback

Event-based music is slightly different from background music, whereas the audio only plays for certain events, such as a bullet being fired or enemy dying etc.

For this to occur, the `AudioStreamPlayer2D` is added to whatever Scene you want the audio to play in/during. For instance, if you wanted audio to play when the player shoots a bullet, the `AudioStreamPlayer2D` would be added to the `Bullet.tscn`, as shown below.

Remember to set the **Autoplay** option to On.

![[audioBulletFire.png]]

