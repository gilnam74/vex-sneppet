# Introduction
**EVE** — out of the box Houdini Pipeline Toolkit under control of Shotgun management system. 

This section is on the early stage of developing, it does not intend to have valuable public content. I mean, **skip it**!

Despite the Houdini data management part is not developed yet, the whole VFX pipeline structure is based on [Animation DNA](https://github.com/kiryha/AnimationDNA/wiki) ideas, so we have a concept for the project folder structure, assets structure etc.

# Documentation
## Project setup
Environment variables allow to setup variables and values in Houdini.  
List all available variables: run `cmds`, run command `hconfig.exe -a`  
Check the variable value in Houdini: in Texport window run `echo $<VAR_NAME>`  
Variables which works with paths divided into 2 types:  
- Location (HIP, JOB, etc), one specific directory
- Path (HOUDINI_SCRIPT_PATH, etc), list of dirs

`JOB` - root for Houdini project.  
`HIP` - path where Houdini scene is.  
`HOME` - Houdini settings for the user  
`HFS` - Houdini install dir  
`HOUDINI_PATH` - Modify (add items) or override standard Houdini behavior. 

#### Check Houdini environment
Run Windows > Shell (which runs `cmds`). Enter `hconfig-ap`

## Project structure
How we sort all project data

#### Film structure
The default structure of the final product (film, clip etc) is:  
**FILM** > **SEQUENCE** > **SHOT**

For bigger projects like animation feature, it has a sense to group sequences into reels:  
**FILM** > **REEL** > **SEQUENCE** > **SHOT**

The default [Shotgun schema](https://support.shotgunsoftware.com/hc/en-us/articles/219031358-Shotgun-schema) match this structure.

#### Folder structure

EDIT — Editing and grading brunch:
- OUT — Output of editing branch, final movie will be rendered into this folder
- PROJECT — Editing software project files (Final Cut, Premiere etc)

PREP — Preproduction brunch

PROD — Production brunch

3D:
- geo [geometry (and other) caches: FBX, ABC, etc]  
- hda [Houdini digital assets]  
- lib [librarys of animations, materials, etc]  

`lib` structure needs to be developed. It does not fit into ASSETS or SHOTS structure scheme.

## Unsorted
#### Animation transfer ANM >> RND
Characters and props: caches (bgeo.sc - geo/SHOTS/010/SHOT_010/ROMA/001/E010_S010_ROMA_001.$F.bgeo.sc)  
Cameras: caches (Alembic - geo/SHOTS/010/010/SHOT_010/CAM/E010_S010_001.abc)  
Environments: HDA (HDA - hda/ASSETS/ENVIRONMENTS/...).  
Animated parts of the ENVIRONMENT has to be stored in different HDA (cant render merged static and animated geo with MB) in ENVIRONMENT_ANM geometry container.
Internal animation in HDA (traffic in a city): caches (bgeo.sc - geo/ASSETS/ENVIRONMENTS/...)  

#### Materials workflow
##### Characters
In the modeling scene $JOB/scenes/ASSETS/CHARACTERS/ROMA/GEO/GEO_ROMA_001.hip load `material library HDA` to the root, rename it to `MATERIALS`. Assign materials from HDA library to asset parts with `material SOP` via groups(create a primitive group for each material, or use groups from transforms in case of alembic SRC geometry: Prim Groups = NameGroup Using Transform Node Name). It will create attribute @shop_materialpath. Export asset geometry to `bgeo.sc`. Load  `material library HDA` and asset geometry to a render scene. Materials assigned automatically. For the ROMA character save material data to a database to be able to convert FBXs from MIXAMO to geo cashes with material assignment.

##### Crowds
Create material stylesheet for a geometry node containing crowds. [Setup materials for character parts](https://c2.staticflickr.com/2/1903/45181271151_8c3d3fda67_o.png) (30_Material Stylesheets.MP4 from Crowds for feature films course): Add Stylesheet parameter > Add Style > Add Target > Add Subtarget > Add Override. Disclaimer: crowds stylesheets does not work as expected, need to use one mat for all agents.

#### Create of render scene
1) Save scene to $JOB/scenes/RENDER/010/SHOT_010/RND_E010_S010_001.hipnc
2) Set shot frame range
3) Create CHARACTERS and ENVIRONMENT geometry nodes. Add ROMA cache to CHARACTERS node, add CITY HDA to ENVIRONMENT node.
4) Create materials HDA, rename to MATERIALS
5) Get Sun and Sky  
6) Get shot camera

#### Lighting data (Sun and Sky) transfer between shots
We can store lights in the library <root3D>/lib/LIGHT as HDA and import this data into the shots. How to transfer lights modifications between shots?

# Pipeline developing notes
Here we will record pipeline developing process

## Naming conventions for files
#### HDA
<hdaName>_001.hda  
<hdaName> = [CITY, CITY_PRX]
#### Render scenes
RND_E010_S010_001.hip
#### Animation scenes
ANM_E010_S010_001.hip
#### FX scenes
<FXCode>_001.hip

## Project Management system
Currently, I consider 3 main option to explore: Ftrack($10/month), Shotgun($30/month), JSON file (Free but hard to edit).

### JSON
There are 2 files ASSETS.json and SHOTS.json in `<rootProject>\PREP\PIPELINE\EVE\database`.
The question is how to setup a list of assets and a list of shots. It could be nested dictionaries (current) or list o dictionaries. Conserns: list of dictionaries may be more close-to Shotgun database type, so it would be more easy to switch from JSON to SG later. But nested dictionaries more easy to work with.

Switch to lists of dictionaries in a joined file DATABASE.json:

{ASSETS: {CHAR: [{code:ROMA, materials:[GEN_BASE:[list of objects]]}, {}], ENV: [], PROPS: []}, SHOTS: {010:[{SHOT_010}]}}

concerns: list of sequences/shots ???

## FX
FX is a special type of animation done by simulation techniques (RBD, particles, fluids etc). To produce FX we need input data from modeling and animation department, eg. models, cameras, animations etc (FX INPUT DATA). The result of FX transferred to render scenes (FX OUTPUT DATA). FX could be done for SHOTS or for ASSETS (to be automatically present in all SHOTS with the particular asset). Each FX should be named <FXName> (in form of 3 letter code?).

#### FX scenes
Houdini scenes with FX are located in $JOB/scenes/FX/ASSETS(SHOTS).

#### FX INPUT DATA
`$JOB/fx/ASSETS(SHOTS)/<FXName>`

#### FX OUTPUT DATA
ASSETS: `$JOB/geo/ASSETS/<assetType>/<assetName>/<FXName>`  
SHOTS:  `$JOB/geo/SHOTS/<episodeCode>/<shotCode>/<FXName>`

## Core concepts
All project data located in one folder on the network drive.  
All project data structured as ASSETS and SHOTS. Shots are the part of one project.  
All project management data and record of project data changes are located in Shotgun.

## Pipeline definition and goals
What is PP  
Pipeline covers:
- Communications  
- Data storage, exchange and tracking


## Agile and Organic Pipeline
There is a well-known VFX production structure with strict vertical hierarchy and tasks division between departments. We well research agile project management option with task division between teams. 

The term "production pipeline" by itself defines our perception of movie making process. Obviously, we see the similarity between movie production and any other manufacturing where we use machines, tools and other mechanical devices. Such conception gives quite successful results.

However, we try to look at production process from the biological point of view and get some interesting ideas. The difference between machine and organism is similar to the difference between nodes and layers — the same task could be achieved differently and perception could define algorithms. Organic pipeline research is our goal here.

We are going to breed shots with EVE...

## Project-pipeline relation
At the highest level, there are two options for defining the relation between a pipeline and projects:
- One pipeline driving all projects  
- Each project has its own pipeline version  

The first option more suitable when you have a limited amount of big and complex long-term projects (like animation feature), second fits better for a bunch of fast tasks, like commercials. Here we will design the first option.

Sure, there is a third option available, a combination of this two methods, when you have one pipeline driving all projects and each project has the ability to customize pipeline individually.

## Pipeline deploy
This is basically pipeline for pipeline developing. Where you keep source code, how you modify it and track changes, how you deliver pipeline to a production for the studio and for outsourcers.

## Run new project.
Download repository as a zip file, extract to any temp location and run `setupProject.bat`. Select directory to hold the project, enter project name and press "CREATE PROJECT" to build a project folder structure with a pipeline. 

## Github developer notes
Install GitHub Desktop. Clone this repo to a PIPELINE folder of a project used to develop the pipeline `E:\256\PROJECTS\NSI\PREP\PIPELINE`.

Create a project in PyCharm, set PIPELINE folder as root.

Exclude PyCharm project folder (.idea) from GitHub commits: go to `PIPELINE/.git/info`, add `.idea` line to "exclude" file.

Modify code, commit changes, Fetch Origin!

## Pipeline structure
Pripeline root folder: `<projectName>/PREP/PIPELINE`  
`runHoudini.py` — Houdini wrapper to launch app. Set Houdini environment  
`createProject.py` — [Create project tool](tools#create-project)  

EVE — folder with pipeline files:
- houdini — folder mapped to HOUDINI_PATH, additional to Houdini install dir.  
`jump.pref`  — add bookmarks to File Open dialog in Houdini.  
`MainMenuCommon.xml` — modify menu, add EVE.
  - toolbar — custom EVE shelves  
  - scripts:
    - 123.py — run when Houdini is launching  
    - 456.py — ren wen open or create a new scene

 
## Paths snippets

$JOB/render/010/SHOT_010/001/E010_S010_001.$F.pic
E:/256/PROJECTS/NSI/PROD/2D/RENDER/010/SHOT_010/E010_S010_v001.mov

# Vector math drafts
Before we deal with **scalar** numbers, which give us a certain **value** (like length, X coordinate, etc). Vector could give us the **direction** and **magnitude**. 

[Subtraction gives the vector between two points ](http://www.leadinglesson.com/subtraction-gives-the-vector-between-two-points)

If we have 2 points, A and B, the representation of vector AB between this points would be the difference in coordinates of points B and A. v@AB = [B.x, B.y, B.z] - [A.x, A.y., A.z]. [SRC](http://tutorial.math.lamar.edu/Classes/CalcII/Vectors_Basics.aspx)