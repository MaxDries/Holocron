---
versionTested:
  - 4.2.2
tags:
  - ISD
  - yr11
---

> [!info] In this tutorial, you will learn how to develop a simple 3D environment. This will be the basis of your FPS.
> This scene will be your 'test' scene, with a basic environment, where you can practice and test your game mechanics.


Open `dev_level.tscn` in the Game folder.

# Create a Floor


To start with, the player will need to walk on something, so the first step is to create a `MeshInstance` which will be the floor of the room where the player starts.

Right-Click on the Game scene and choose Add Child Node. Search for `MeshInstance3D.`

![[fpsEnvironment-CreateNewNode.png|Create New Node]]

Rename the node “Floor” or “Ground” or whatever is appropriate. In the Inspector, look for the Mesh attribute. in the dropdown box, choose `New PlaneMesh`.

![[fpsEnvironment-FloorRename.png|Rename Floor]]


At this stage, the mesh is there, but the player (created later) will fall straight through it.

![[fpsEnvironment-AddPlaneMesh.png|Add Plane Mesh]]

Currently, the floor is quite small. The player object that will be created later will be quite significantly larger, so the relative scale will need to be addressed.

Edit the mesh.

![[fpsEnvironment-EditMesh.png|Edit The Mesh]]

Set the `x` and `y` size values to something larger. In this case, 20 has been used. This may need to be modified at a later date.

![[fpsEnvironment-ResizePlaneMesh.png|Resize Plane Mesh]]

With the mesh selected, go to the Mesh Menu and choose Create `Trimesh Static Body`.

![[fpsEnvironment-CreateTriMesh.png|Create TriMesh]]

This updates the mesh to include 2 child nodes.

![[fpsEnvironment-TriMeshNewNodes.png|New Nodes Created]]

![[commonBlocks#Commit & Push]]

# Texture the floor


> [!note] A **Texture** is simply an image that’s applied to a mesh.

Find an image to suit the environment appropriate for the game. 

> [!tip] When searching for images, add the word “seamless” to your searches. This means that the edges of the image align with the opposite side, meaning that when the image is tiled on a mesh, no edging will be visible.

Store the image in a folder created for Images.

![[fpsEnvironment-CreateImagesFolder.png|Create Images Folder]]

Select the `Floor` node. In the inspector, expand out the Material dropdown.

![[fpsEnvironment-ExpandMaterial.png|Expand to show Material]]

There is currently no material attached, hence why the mesh is white. Click on the dropdown next to [empty] and choose `New StadardMaterial3D`.

![[fpsEnvironment-NewStandardMaterial3D.png|New StandardMaterial3d]]

![[fpsEnvironment-NewStandardMaterial3D2.png|New StandardMaterial3D]]

Click on the white sphere that appears. In the menu that appears, expand `Albedo`. 


> [!info] Albedo is the default type for textures. There are many more as you can see in the list.


![[fpsEnvironment-MaterialAlbedo.png|Albedo]]

Drag the texture from the FileSystem tab to the Texture option under Albedo. The texture on the plane has been updated.

![[fpsEnvironment-ApplyMaterial.gif|Apply Texture to Material]]

> [!note] This is the same process to texture any of your other assets, unless they’ve been textured prior to importing.

The texture can be ‘tiled’ instead of stretched, by editing the x and y values under `Uv 1`. 

This avoids the image being stretched the fit the size of the plane. s

Choose values that suit the needs of the game and the desired effect. I.e. set the tiles value as high or as low as you wish to give the detail level you want.

![[fpsEnvironment-MaterialTiling.png|Tiling]]

# Add Lighting

The scene at this stage is too dark for the player. Right-click on the Root Node, choose Add Child Node, search for and add a `DirectionalLight3D`.

![[fpsEnvironment-DirectionalLight.png|Create Directional Light]]

With the `DirectionaLight3D` selected, enable Shadows in the Inspector.

Set the Rotational Degrees so that the light is roughly coming down from ‘the sky’ and creates the appropriate shadows.

The position of the directional light is not relevant, however the arrow shown in the image shows the direction the light will travel. 

![[fpsEnvironment-DirectionalLightDirection.png|Set Direction of Light]]

# Build your Environment

Find, import and put your environment models into your scene.

You may wish to add walls, or fences. 

At this stage, keep it simple and get a good example of your first scene. 

Similar to the Main Menu, the environment can be further developed at a later stage of the development process.

![[commonBlocks#Commit & Push]]