# Introduction
I hope nobody doubts that the ability to write even simple scripts it's a huge benefit for any CG artist. Why then it's not that common for artists to be able to code? Probably there is only one reason besides laziness: people thinking that coding is hard and requires a special gift. But this is not the case, anybody can build programs that will make your life way easier.

Try to follow these tutorials and you will get the knowledge on how to build your own working tools!

When dealing with a programming we should decide which language we would use to achieve our goals first. And choosing the proper language sometimes is a really sophisticated task required a huge experience in computer science. Fortunately for CG artist, this choice has been already made by the developer of your favorite software. Lucky users of Maya, Nuke, Houdini and a lot of other major DCC (hello, Adobe!) support Python. And this is super awesome great because Python is very easy to learn and use.

[Basics](#python-in-houdini)  |  [Creating a first custom tool](#creating-a-first-custom-tool)

## Python integration basics
You don't need to do any extra step to start writing Python code in Houdini, Python already integrated there.

Python integrated in Houdini in several ways:
- Windows > Python Shell - Python shell for small scripts/commands  
- Windows > Python Source Editor - Python editor which saves the code with *.hip file. Each time you open the file Python code in Source Editor will be executed. Best option to develop basic chanks of code and test their functionality. To use Python Scripts in all the scenes you need to save them to the Shell and run from there.    
- In the parameters fields of any node (switch to Python from HScript).  
- Python node in SOP context - This is how you can use Python in your nodes graph to create and modify geometry.
- In assets - create asset, right-click > Type properties > Scripts tab.
- Python Panel - to create PySide UI

For the most types of tasks here, we will use `Python Source Editor`. It allows you to write and execute Python code in the current Houdini scene. If you are completely new to programming I recommend you to read [Programming Basic](https://github.com/kiryha/Houdini/wiki/Programming-basics) tutorial first.

Launch a `Python Source Editor` and type a code in the top window:

```python
print 'Hello, World'
```
Press the `Apply` button, Houdini Console window should appear with a printed message  'Hello, World'. [Was it that hard](Programming-basics#programming-learning-curve)?

To be able to write real programs and scripts for Houdini, besides Python integration, we would need the ability to access Houdini scene components (to query and modify necessary data). This ability provided by Application Programm Interface (API). Houdini Python API exists as **HOM** - Houdini Object Model. To use this module in Houdini we need to import it to our python code:

```python
import hou
```
Let's extend our first Hello World program a bit by using built-in UI in Houdini Python API:

```python
import hou
hou.ui.displayMessage('Hello, World')
```

## Creating a first custom tool
Here we will create from scratch a custom tool (lets name it GeoCreator) for Houdini with a User Interface. GeoCreator will generate an empty geometry node with a certain name in the root of the current scene. Not an outstanding functionality but it's just a basic example which will help us to start solving tasks in Houdini with Python. Besides a huge amount of video tutorials starts from creating a geometry node, diving inside, deleting File node and creating a grid, while you can do the same by CTR + click on Grid shelf tool. So might be useful in some cases.

We will begin with building a functional part of the code in the Python Source Editor, then we will add and setup UI and finally we will save the code as a Python file and make it launch from the Shelve.

### Develop functionality
Let's design a Python program which will solve the task of creating a geometry node with a specific name. 

To create a node in Houdini with Python use a `createNode()` command. To run this command successfully we need two main things: 
- A [string](Programming-basics#data-types) [argument](Programming-basics#commands) which will define the type of node which will be created. 
- A parent object, where this node will be placed (). In case

The most obvious way to get the name of a required node type is to create this node with UI and take the name from Info Box window. Run Houdini, CTR + click on the Grid Tool in the Create Shelf tab and open Info Box for `grid_object1` node. 
[![](https://c2.staticflickr.com/2/1907/44985678922_808fbd207a_o.png)](https://c2.staticflickr.com/2/1907/44985678922_808fbd207a_o.png)

The type of node is written in brackets under the object name: Geometry Object (geo), so the node type we need to create is `geo`. Delete the grid_object1 and open a Python Source Editor. 
 
In our case, parent object would be a root of the object context: `hou.node('/obj/')`. 

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
geometry.setName('MY_GEO')
```

Run the code and get empty geometry node named `MY_GEO`. Great

Now the code is being executed line by line from the top to the bottom.

Code structuring. Create run() function

### Create and setup UI
One of the possible workflows for building tools with UI in Houdini is creating interfaces in QT Designer (shipped with Python27) and importing *.ui files into your Python code where you will develop the tool functionality.

Build a UI with a single button. Rename button object to `btn_create`.

### Run tool from the Shell