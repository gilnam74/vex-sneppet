# Introduction
I hope nobody doubts that the ability to write even simple scripts it's a huge benefit for any CG artist. Why then it's not that common for artists to be able to code? Probably there is only one reason besides laziness: people thinking that coding is hard and requires a special gift. But this is not true, anybody can build programs that will make your life way easier.

Try to follow these tutorials and you will get the knowledge on how to build your own working tools!

[Basics](#python-in-houdini)  |  [Creating custom tool with UI]()

## Python integration basics
Python integrated in Houdini in several ways:
- Windows > Python Shell - Python shell for small scripts/commands  
- Windows > Python Source Editor - Python editor which saves the code with *.hip file. Each time you open the file Python code in Source Editor will be executed. Best option to develop basic chanks of code and test their functionality. To use Python Scripts in all the scenes you need to save them to the Shell and run from there.    
- In the parameters fields of any node (switch to Python from HScript).  
- Python node in SOP context - This is how you can use Python in your nodes graph to create and modify geometry.
- In assets - create asset, right-click > Type properties > Scripts tab.
- Python Panel - to create PySide UI

For the most types of tasks here, we will use `Python Source Editor`. It allows you to write and execute Python code in the current Houdini scene. If you are completely new to programming I recommend you to read [Programming Basic](https://github.com/kiryha/Houdini/wiki/Programming-basics) tutorial first.

To be able to write programs and scripts for Houdini, besides Python integration, we would need the ability to access Houdini scene components (to query and modify necessary data). This ability provided by Application Programm Interface (API). Houdini Python API exists as **HOM** - Houdini Object Model. To use this module in Houdini we need to import it to our python code:

```python
import hou
```

## Create custom Python tool with UI
Here we will create from scratch custom tool for Houdini with UI. One of the possible workflows for building tools with UI in Houdini is creating interfaces in QT Designer (shipped with Python27) and importing *.ui files into your Python code where you will develop the tool functionality.

### Intro
This tool will create an empty geometry node with a certain name in the root of the current scene. Not an outstanding functionality but quite nice to get started to build your own custom tools. Besides a huge amount of video tutorials starts from creating a geometry node, diving inside, deleting File node and creating a grid, while you can do the same by CTR + click on Grid shelf tool. So might be useful in some cases.

We will run the code from Python Source Editor while developing necessary functionality. After tool would be ready we will set up launching it as a Shelf Tool.

First, let's develop a functional part of the code. Run Python Source Editor. To create any node in Houdini with Python you need to define a parent for this node. In our case, parent object would be a root of the object context: `hou.node('/obj/')`. Then you can create a geometry node setting this object as a parent.

```python 
import hou

# Get scene root node
OBJ = hou.node('/obj/')
# Create Geometry node in scene root
geometry = OBJ.createNode('geo')
```
Run the code and you will get the Geometry node named 'geo1' in the scene root with file node inside. To name the newly created object according to our needs we would use `setName()` command. To get rid of the file node we would set to False `run_init_scripts` flag for `createNode()` command.

```python 
import hou

# Get scene root node
OBJ = hou.node('/obj/')
# Create Geometry node in scene root
geometry = OBJ.createNode('geo', run_init_scripts=False)
# Set Geometry node name
geometry.setName('GEO')
```

Code structuring. Create run() function

Build a UI with a single button. Rename button object to `btn_create`.