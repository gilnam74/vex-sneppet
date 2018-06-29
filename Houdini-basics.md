# Introduction
Here you will find Houdini general usage tips

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

## Attributes and parameters
[Attributes](http://www.sidefx.com/docs/houdini/model/attributes)  
[Global expression variables](http://www.sidefx.com/docs/houdini14.0/expressions/_globals)  
[Standard variables](http://www.sidefx.com/docs/houdini/nodes/sop/standardvariables)  
[Local SOP variables](http://www.sidefx.com/docs/houdini/nodes/sop/point#locals)

## Hotkeys: Scene view
- Reset rotate pivot to a viewport center: `Shift` + `Z`

## Hotkeys: Network view
- **Align** nodes: press `A`, press LMB, move left(or up)  
- **Move** node with **upstream** connections: `Shift` + `LMB` + move  
- **Move** node with **downstream** connections: `Ctrl` + `LMB` + move
- **Duplicate** node: `Alt` + `LMB` + move . 
- **Duplicate** node with **upstream** connections: `Alt` + `Shift` + `LMB` + move  
- **Duplicate** node with **downstream** connections: `Alt` + `Ctrl` + `LMB` + move  
- Set **display and render** flag: `R` + `LMB` click   
- Set **render** flag: `T` + `LMB` click   
- Set **bypass** flag: `Q` + `LMB` click  
- Set **template** flag: `W` + `LMB` click  
- **Reoder** inputs: `Shift` + `R`  
- Add **dot**: `Alt` + `LMB` click . 
- **Pin** the wire: `Alt` + `LMB` click  
- **Cut** the wire: `Y` + `LMB` drag  
- **Set** quickmark 1: `Ctr` + `1`  
- **Go to** quickmark 1: `1`  
- **Togle** quickmarks: tilda

## Pipeline developing notes
#### Introduction
This repository is a Houdini pipeline toolset in the early stage of developing. Working name is "Mother". This section is my notes on developing process, it does not intend to have valuable public content. I mean, skip it!

Once released, Mother would be **out of the box VFX pipeline** for Houdini and Nuke applications under control of Shotgun management system.

Despite the Houdini data management part is not developed yet, the whole VFX pipeline structure is based on [Animation DNA](https://github.com/kiryha/AnimationDNA/wiki) ideas, so we have a concept for the project folder structure, assets structure etc.

#### Project-pipeline relation
At the highest level, there are two options for defining the relation between a pipeline and projects:
- One pipeline driving all projects  
- Each project has its own pipeline version  

The first option more suitable when you have a limited amount of big and complex long-term projects (like animation feature), second fits better for a bunch of fast tasks, like commercials. Here we will design the first option.

#### Pipeline deploy
This is basically pipeline for pipeline developing. Where you keep source code, how you modify it and track changes, how you deliver pipeline to a production for the studio and for outsoursers.

#### Usage
Create a project [folder structure](https://github.com/kiryha/AnimationDNA/wiki/02-Codex-DNA#folder-structure) and place repo content in `<rootProject>/PREP/PIPELINE` to use Mother.

#### Github developer notes
Install GitHub Desktop. Clone this repo to a PIPELINE folder of a project used to develop the pipeline `E:\256\PROJECTS\NSI\PREP\PIPELINE`.

Create a project in PyCharm, set PIPELINE folder as root.

Exclude PyCharm project folder (.idea) from GitHub commits: go to `PIPELINE/.git/info`, add `.idea` line to "exclude" file.

Modify code, commit changes, Fetch Origin!