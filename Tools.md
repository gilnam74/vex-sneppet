# Introduction
This page contains a user guide for all available tools in Mother Houdini Pipeline Toolkit.

## Project Management
Creating and managing projects 

#### Create Project
Create Project tool allows to setup and run a new project with Mother. Run `createProject.bat`, Create Project main window will rise:

[![](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)

[ SET LOCATION ] — Folder to store all project data  
[ CREATE PROJECT ] — Run the project creation process. This will add a folder with a folder structure, copy pipeline files inside, create a project in Shotgun based on user-defined settings.

## Unsorted
Will be sorted after getting a descent amount of tools
#### Save Next Version
`SNV` button on EVE shelf. Incrementaly saves current houdini scene (<fileCode>_001.hip >> <fileCode>_002.hip).

If a file with next version exists, tool finds the latest existing version and rise warning window with such options:  
- Save Next Version — save the file with the latest available version  
- Overwrite Existing — overwrite file with latest existing version


[![](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)

#### Create Scene
`Create Scene` button on EVE shelf. Create animation or render scene. WIP. Heavily rely on the database (Shotgun)

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