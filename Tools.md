# Introduction
This page contains a user guide for all available tools in Mother Houdini Pipeline Toolkit.

## Project and data management
Creating and managing projects 

#### Create Project
Create Project tool allows to setup and run a new project with Mother. Run `createProject.bat`, Create Project main window will rise:

[![](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)

[ SET LOCATION ] — Folder to store all project data  
[ CREATE PROJECT ] — Run the project creation process. This will add a folder with a folder structure, copy pipeline files inside, create a project in Shotgun based on user-defined settings.

#### Save Next Version
`SNV` button on EVE shelf. Incrementaly saves current houdini scene  
fileCode_001.hip >> fileCode_002.hip

If a file with next version exists, tool finds the latest existing version and rise warning window with such options:  
- Save Next Version — save the file with the latest available version  
- Overwrite Existing — overwrite a file with latest existing version  
- Cancel — cancel saving file

#### Create Scene
`Create Scene` button on EVE shelf. Create animation or render scenes. Heavily rely on the database (Shotgun). At the current stage, the database exists as a JSON file which user has to edit manually in a text editor to setup assets and shots data. 

The script creates render scene for desired shot: 
- Name and save Houdini file
- Import lights and materials via HDA
- Creates assets based on shot data: environment, characters, props and FXs.
- Add Mantra node and set its parameters (sampling and output file)

## Unsorted
Will be sorted after getting a descent amount of tools



[![](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)



#### Create Flipbook
`FB` button on EVE shelf. Create Flipbook for current animation or render scene.

#### import ANM
Import animation caches to a render scene. WIP.

## Temporary
Tools currently used for "NSI" project. Will be removed for the final pipeline.
#### Import FBX
#### Save MATS

#### Convert FBX
`FBX>GEO` button on EVE shelf. Converts imported to a Houdini scene FBX files with a character animations from Mixamo to a Geometry Cache. For each selected FBX Subnetwork node creates:
- a subnetwork with merged from FBX geometries,  
- a material group for each part and apply a material to each geometry via material SOP,
- File Cache node to write the character animation caches to an Animation library