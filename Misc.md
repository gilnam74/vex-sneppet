# Houdini  
[Project](#project-setup)  |  [16](#houdini-16)  | [ATTR](#attributes-and-parameters) |  [SOP](#sop)  | [Dynamic](#dynamic) | [Rendering](#rendering) | [Expressions](#expressions) |  [VEX](#vex) | [Glossary](#glossary)

## Project setup
### Houdini variables
Environment variables allow to setup variables and values in Houdini.  
List all available variables: `hconfig.exe -a`  
Check variable value in Houdini: in Texport window run `echo <$VAR_NAME>`  
Variables which works with paths divided into 2 types:  
- Location (HIP, JOB, etc)
- Path (HOUDINI_SCRIPT_PATH, etc)

`$JOB` - root for Houdini project.  
`$HIP` - path where Houdini scene is.  
`HOME` - Houdini settings for user

## Attributes and parameters
[Attributes](http://www.sidefx.com/docs/houdini/model/attributes)  
[Global expression variables](http://www.sidefx.com/docs/houdini14.0/expressions/_globals)  
[Standard variables](http://www.sidefx.com/docs/houdini/nodes/sop/standardvariables)  
[Local SOP variables](http://www.sidefx.com/docs/houdini/nodes/sop/point#locals)


## SOP
#### Space colonization algorithm
Download [Space Colonisation hip file](../blob/master/hips/spaceColonization_001.hipnc)
[![](https://c1.staticflickr.com/1/797/39601799870_0e3dfe55b3_o.gif)](https://c1.staticflickr.com/1/797/39601799870_0e3dfe55b3_o.gif)

#### Randomize copies