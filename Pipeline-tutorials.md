# Intro
This section contains `Eve pipeline` related tutorials.

## Installation
Currently `Eve` designed to be run on Windows OS. Here is the list of things you need to have before pipeline will works.

Before running Eve pipeline you need to install:
- Houdini 
- Python 2.7.5 x 64 (or later version) 
- pip  
- PySide 2
- GitHub Desktop (optionally)

#### Install Houdini
Go to [SESI site](https://www.sidefx.com/products/compare/), choose, download and install your Houdini version. Eve assumes Houdini would be located in:

`C:/Program Files/Side Effects Software/Houdini <buildNumber>/bin/houdinifx.exe`,  

otherwise, you will need to tweak this path in `Eve/tools/core/settings.py` before creating any projects. 

#### Install Python
Get your [X64 Python 2.7.5](https://www.python.org/downloads/release/python-275/)

#### Install pip
To install PIP do the folowing:
* Download [get-pip.py](https://bootstrap.pypa.io/get-pip.py), place in Python folder, e.g `C:/Python27/` 
* Run cmd in `C:/Python27` folder and execute: `python get-pip.py`  
* Add pip path to a system environment variables: `PATH = c:/Python27/scripts`  

#### Install Pyside 2 for Python 27
Tricky part. If you don't know how to do this you can just [download Pytho27 archive](https://drive.google.com/file/d/1jC4x2-Dcf5saixe9Z5aBu-kIMMaGEmtJ/view?usp=sharing) with all necessary libraries and place it on  `C:/` drive. 

Here is the [Pyside 2 for Python 27 wheel file](https://drive.google.com/file/d/1bJv2IM40QC8D8w7pFw1DKVYUSoSiFqk4/view?usp=sharing).

#### Install GitHub Desktop and clone pipeline
Install [Github Desktop](https://electronjs.org/apps/github-desktop) and clone this repo to the hard drive where you suppose to keep Eve pipeline, for example, `D:/Eve`. 

[![](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)

Alternatively, you can just download this repo as a zip file and extract to the same desired location. Hence you would not need GitHub Desktop but, in this case, it would be more tricky to get updates each time (download again, extract, overwrite existing files). 

 
## The first Eve project
The information below is out of date since Eve was restructured in July 2020.

Here we will step by step create an example project to understand the basics of Eve workflow. 
[![](https://live.staticflickr.com/65535/48087767582_928942e9c1_o.jpg)](https://live.staticflickr.com/65535/48087767582_928942e9c1_o.jpg)

#### Project description
Originally Eve was designed to create a full CG [music video](https://www.youtube.com/watch?v=NaXP6afe6R4) for a song called "Beauty". We will extremely simplify this project and reproduce all steps to create a final look. 

We will have three assets:
- Character asset "ROMA"
- Environment asset "KIEV"
- FX asset "VANISH"

Those assets would be used in one shot. 

#### Create project with Eve
`Eve` presented as a folder with files on your hard drive and it does not include your project's data. For each of your project, we will need an additional folder usually located somewhere in "projects" directory, for example, `D:/projects/<projectName>`. Those project folders would be created automatically with Eve `Create Project` tool.

Go to the Eve root folder `P:/Eve` and run `createProjecrt.bat`:

[![](https://live.staticflickr.com/65535/48019770601_10f9642217_o.gif)](https://live.staticflickr.com/65535/48019770601_10f9642217_o.gif)

Select the project location (`P:/projects/`), enter the project name (`beauty`), Houdini build number you have installed, check "Copy Example Data" and press the "Create Project" button. If everything went right, Windows Explorer will open a folder with a Houdini wrapper file (`runHoudini.bat`) located in `P:/projects/beauty/PREP/PIPELINE/`. Create a shortcut on Desktop to this file and launch Houdini by double-clicking it.

"Copy Example Data" checkbox allows to copy all Eve tutorials files into the new project, so if you will not be able to follow any step, you always can examine provided materials.

#### Create assets and shots in the database
Before dive deep into Houdini magic, we need to prepare some data. Run `Project Manager` tool (`PM` button on Eve shelf) and create character asset "ROMA", environment asset "KIEV", FX asset "VANISH", sequence 010 and SHOT_010.

[![](https://live.staticflickr.com/65535/48056687948_124c55d2fe_o.gif)](https://live.staticflickr.com/65535/48056687948_124c55d2fe_o.gif)

#### Create asset files
Now let's make hip files for each of asset.

##### KIEV
Select "KIEV" asset and press "Create Hip" button in Project Manager. 

The scene with the empty "KIEV" Houdini Digital Asset would be created in:

`<root3D>/scenes/ASSETS/ENVIRONMENTS/KIEV/ENV_KIEV_001.hiplc`. 

Houdini Digital Asset would be saved to:

`<root3D>/hda/ASSETS/ENVIRONMENTS/KIEV/HDA_KIEV_001.hdalc`. 

Houdini scene is a working file where you suppose to create all elements of the environment asset. Houdini Digital Asset "KIEV" is a container to store resulting models (which meant to be used in animation and render scenes). Usually, it will contain baked geometry and volumes to represent final environment look as well as UI elements to control asset behavior. Lights? 

Here is an example of environment asset UI:

[![](https://live.staticflickr.com/65535/48088489032_28baa44950_o.gif)](https://live.staticflickr.com/65535/48088489032_28baa44950_o.gif)

If you checked "Cope Example Project" while creating this project, the environment asset scene and environment asset HDA would be already in place, so the script will ask you if you want to create new versions of `ENV_KIEV_001.hiplc` and `HDA_KIEV_001.hiplc`. Save the next versions of files.

Since we use simplified data, just create a geometry node inside "KIEV" HDA and load `<root3D>/caches/ASSETS/ENVIRONMENTS/KIEV/terrain_001.bgeo.sc` with File Cache node. Save "KIEV" HDA (right click > Save Node Type). 

Open Project Manager, select KIEV asset, enter HDA name `KIEV`and press "Save Asset Edits"

Use "Save Next Version" tool to save the scene to the next version when needed.

##### ROMA
Select "ROMA" asset and press "Create Hip" button in Project Manager. 

The empty scene would be created in:
`<root3D>/scenes/ASSETS/CHARACTERS/ROMA/GEO/GEO_ROMA_001.hiplc`

Create "ROMA" geometry node and load character geometry with FileCache node from:
`<root3D>/caches/ASSETS/CHARACTERS/ROMA/GEO/GEO_ROMA_001.bgeo.sc`

#### VANISH
Select "VANISH" asset and press "Create Hip" button in Project Manager. 

The scene with empty "VANISH" Houdini Digital Asset would be created in:
`<root3D>/scenes/FX/ASSETS/ENVIRONMENTS/VANISH/FXS_VANISH_001.hiplc`

The HDA with VANISH effect would be saved in:
`<root3D>/hda/FX/ASSETS/ENVIRONMENTS/VANISH/HDA_VANISH_001.hdalc`

Open Project Manager, select VANISH asset, enter HDA name `VANISH` and press "Save Asset Edits"