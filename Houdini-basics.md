# Introduction
Here you will find Houdini general usage tips

## Project setup
Environment variables allow to setup variables and values in Houdini.  
List all available variables: `hconfig.exe -a`  
Check the variable value in Houdini: in Texport window run `echo $<VAR_NAME>`  
Variables which works with paths divided into 2 types:  
- Location (HIP, JOB, etc)
- Path (HOUDINI_SCRIPT_PATH, etc)

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

