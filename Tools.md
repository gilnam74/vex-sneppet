# Introduction
This page contains a user guide for all available tools in Eve Houdini Pipeline Toolkit.

## Project and data management
Creating and managing projects 

#### Create Project
Create Project tool allows to setup and run a new project with Eve. Run `createProject.bat`, Create Project main window will rise:

[![](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)](https://c2.staticflickr.com/2/1801/42501789174_f5af9c9462_o.gif)

[ SET LOCATION ] — Folder to store all project data  
[ CREATE PROJECT ] — Run the project creation process. This will add a folder with a folder structure, copy pipeline files inside, create a project in Shotgun based on user-defined settings.

#### Project Manager
Once you have your project in place its time to create assets and shots and make connections between them. Project Manager is a sort of light Shotgun, a system which controls your pipeline data.

[![](https://live.staticflickr.com/65535/48056687948_124c55d2fe_o.gif)](https://live.staticflickr.com/65535/48056687948_124c55d2fe_o.gif)


#### Save Next Version
`SNV` button on EVE shelf. Incrementaly saves current houdini scene  
fileCode_001.hip >> fileCode_002.hip

[![](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)](https://c2.staticflickr.com/2/1915/45102596111_6576562e3a_o.gif)

If a file with next version exists, tool finds the latest existing version and rise warning window with such options:  
- Save Next Version — save the file with the latest available version  
- Overwrite Existing — overwrite a file with latest existing version  
- Cancel — cancel saving file

#### Create Scene
`Create Scene` button on EVE shelf. Create animation or render scenes. Heavily rely on the database (Shotgun). At the current stage, the database exists as a JSON file which user has to edit manually in a text editor to setup assets and shots data. 

[![](https://c2.staticflickr.com/8/7874/46370254914_29e08155e0_o.gif)](https://c2.staticflickr.com/8/7874/46370254914_29e08155e0_o.gif)

The script creates render scene for desired shot: 
- Name and save Houdini file
- Import lights and materials via HDA
- Creates assets based on shot data: environment, characters, props and FXs.
- Add Mantra node and set its parameters (sampling and output file)

#### Export Animation
`Export ANM` button on EVE shelf. Exports data from animation to the render scene:
- Export shot camera to Alembic
- Export character caches

#### Import Animation
`Import ANM` button on EVE shelf. Import animation data in render scene including FXs:
- Import shot camera from Alembic
- Import character caches
- Import FXs (TBD)

#### Render Farm
`Farm` button on EVE shelf. Process recursive rendering of a list of hip files until all frames will be complete. Add shots to the list via sequence and shot numbers. Before publishing will be implemented, assumes that the latest render scene outputs EXR to the latest rendering folder version. 

[![](https://live.staticflickr.com/65535/33872484668_f1470dcb89_o.gif)](https://live.staticflickr.com/65535/33872484668_f1470dcb89_o.gif)

## Unsorted
Will be sorted after getting a decent amount of tools

#### Create Flipbook
`FB` button on EVE shelf. Create Flipbook for current animation or render scene.



## Temporary
Tools currently used for "NSI" project. Will be removed for the final pipeline.
#### Import FBX
#### Save MATS

#### Convert FBX
`FBX>GEO` button on EVE shelf. Converts imported to a Houdini scene FBX files with a character animations from Mixamo to a Geometry Cache. For each selected FBX Subnetwork node creates:
- a subnetwork with merged from FBX geometries,  
- a material group for each part and apply a material to each geometry via material SOP,
- File Cache node to write the character animation caches to an Animation library