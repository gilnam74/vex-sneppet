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
Here we will create from scratch a custom tool (let's name it `GeoCreator`) for Houdini with a User Interface. GeoCreator will generate an empty geometry node with a certain name ("MY_GEO") in the root of the current scene. Not an outstanding functionality but it's just a basic example which will help us to start solving tasks in Houdini with Python. Besides a huge amount of video tutorials starts from creating a geometry node, diving inside, deleting File node and creating a grid, while you can do the same by CTR + click on Grid shelf tool. So might be useful in some cases.

We will begin with building a functional part of the code in the Python Source Editor, then we will add and setup UI and finally we will save the code as a Python file and make it launch from the Shelve.

### Develop functionality
Let's design a Python program which will solve the task of creating a geometry node with a specific name. 

To create a node in Houdini with Python use a `createNode()` command. To run this command successfully we need two main things: 
- A [string](Programming-basics#data-types) [argument](Programming-basics#commands) which will define the type of node which will be created. 
- A parent [object](Programming-basics#arguments-and-objects), where this node will be created: `parentObject.createNode()`.

The most obvious way to get the name of a required node type is to create this node with UI and take the name from Info Box window. Run Houdini, CTR + click on the Grid Tool in the Create Shelf tab and open Info Box for `grid_object1` node. 
[![](https://c2.staticflickr.com/2/1907/44985678922_808fbd207a_o.png)](https://c2.staticflickr.com/2/1907/44985678922_808fbd207a_o.png)

The type of node is written in brackets under the object name: Geometry Object (geo), so the node type we need to create is `geo`.

The node created with `parentObject.createNode()` command will be placed inside the parent object. In the case of scene root, the parent object will be the scene context. To create a Python object from the object in the scene use `hou.node()` command and give a path to object in the scene as an argument to this command. In case of scene root, it would be `hou.node('/obj/')`.


Delete the grid_object1, open a Python Source Editor and type the code: 
 
```python 
import hou

# Get scene root node
sceneRoot = hou.node('/obj/')
# Create Geometry node in scene root
sceneRoot.createNode('geo')
```
Run the code and you will get the Geometry node named 'geo1' in the scene root with file node inside. To give a certain name to the newly created node we have 2 options:
- pass the desired name as a string argument after the node type argument in `createNode()` command. We will use this option first. 
- rename node after it has been created with `setName('<objectName>')` command.

To get rid of the file node we would set to False `run_init_scripts` flag for `createNode()` command.

```python 
import hou

# Get scene root node
sceneRoot = hou.node('/obj/')
# Create empty "MY_GEO" geometry node in scene root
sceneRoot.createNode('geo', 'MY_GEO', run_init_scripts=False)
```

Run the code and get empty geometry node named `MY_GEO`. Great, the code is working and produce the result we aimed to achieve! If it was a real task from the production experience we could stop at this point and just use this code when we need to create "MY_GEO" geometry node. But since we are building a `GeoCreator` Shelf Tool with UI we will need some more work to make it done. 

Let's use another naming option, rename object after creation. To create a Python object we just need to store the result of createNode() command as a variable. Then you can use this object to apply `setName()` command:

```python
import hou

# Get scene root node
sceneRoot = hou.node('/obj/')
# Create empty geometry node in scene root
geometry = sceneRoot.createNode('geo', run_init_scripts=False)
# Set geometry node name
geometry.setName('MY_GEO')
```

Delete the existing "MY_GEO" node and run the code. We have the same result again, "MY_GEO" node in the root of the scene. Try to run this code again and you will get an error. One more geometry node ("geo1") will be created in the scene but after creation, a script would not be able to rename the node to "MY_GEO" because the node with such a name already exists in our scene. If we run the script several times using the first naming option (pass name as an argument to a `createNode()` command) we would not get an error, Houdini just add a number to an existing name (MY_GEO1, MY_GEO2, etc) which is not that bad as an error but not what we need to get as well. 

Either way, we have to extend the functionality of the GeoCreator tool to be able to behave differently if the object with a required name already exists. One option to solve this is trying to create an object from "MY_GEO" node with `hou.node()` command passing the path to "MY_GEO" as an argument and then check if this object does not have a value of `None`: 

```python
import hou

# Get scene root node
sceneRoot = hou.node('/obj/')
# Check if "MY_GEO" exists
if hou.node('/obj/MY_GEO') == None:
    # Create empty geometry node in scene root
    geometry = sceneRoot.createNode('geo', run_init_scripts=False)
    # Set geometry node name
    geometry.setName('MY_GEO')
```

Now the script will create "MY_GEO" node only if it does not exists in the scene already. If the "MY_GEO" node exists GeoCreator will do nothing and it could be frustrating for the end users. Scripts always should inform users about program execution. Usually, it could be print statements (seeing in Houdini Console).  We will raise the window with the appropriate message if the "MY_GEO" node exists or has been created:

```python
import hou

# Get scene root node
sceneRoot = hou.node('/obj/')
# Check if "MY_GEO" exists
if hou.node('/obj/MY_GEO') == None:
    # Create empty geometry node in scene root
    geometry = sceneRoot.createNode('geo', run_init_scripts=False)
    # Set geometry node name
    geometry.setName('MY_GEO')
    # Display creation message
    hou.ui.displayMessage('MY_GEO node created!')
else:
    # Display fail message
    hou.ui.displayMessage('MY_GEO already exists in the scene')
```

Now let's organize our code a bit better. It may seem redundant to organize several lines of code but we do this for the learning purposes, so you will understand how to build more complex tools in the future and it becomes necessary later when we will start building UI.

What issue should be obvious to improve is that we use the same mane "MY_GEO" several times in our code. Despite it is a static name we are not going to change it by the program brief it is always better to build a code which is easy to edit and read. Common practice for such cases is to create a variable with a required name once and then use this variable in code as many time as you need later:

```python
import hou

# Define geometry node name
geometryName = 'MY_GEO'

# Get scene root node
sceneRoot = hou.node('/obj/')
# Check if "MY_GEO" exists
if hou.node('/obj/{}'.format(geometryName)) == None:
    # Create empty geometry node in scene root
    geometry = sceneRoot.createNode('geo', run_init_scripts=False)
    # Set geometry node name
    geometry.setName(geometryName)
    # Display creation message
    hou.ui.displayMessage('{} node created!'.format(geometryName))
else:
    # Display fail message
    hou.ui.displayMessage('{} already exists in the scene'.format(geometryName))
```
Here we use **Pyhon string formatting** to replace `{ }` with value from variable:
```python
print 'The fruits are {0}, {1}, {2}!'.format('Apple', 'Banana', 'Kiwi')
>> Numbers are Apple, Banana, Kiwi!
```
 
Now lets wrap a part of our program to a [function](Programming-basics#functions):

```python
import hou

# Define geometry node name
geometryName = 'MY_GEO'

def createGeoNode():
    # Get scene root node
    sceneRoot = hou.node('/obj/')
    # Check if "MY_GEO" exists
    if hou.node('/obj/{}'.format(geometryName)) == None:
        # Create empty geometry node in scene root
        geometry = sceneRoot.createNode('geo', run_init_scripts=False)
        # Set geometry node name
        geometry.setName(geometryName)
        # Display creation message
        hou.ui.displayMessage('{} node created!'.format(geometryName))
    else:
        # Display fail message
        hou.ui.displayMessage('{} already exists in the scene'.format(geometryName))

# Execute node creation 
createGeoNode()
```

And pass the name of geometry to our function explicitly:

```python
import hou

# Define geometry node name
name = 'MY_GEO'

def createGeoNode(geometryName):
    # Get scene root node
    sceneRoot = hou.node('/obj/')
    # Check if "MY_GEO" exists
    if hou.node('/obj/{}'.format(geometryName)) == None:
        # Create empty geometry node in scene root
        geometry = sceneRoot.createNode('geo', run_init_scripts=False)
        # Set geometry node name
        geometry.setName(geometryName)
        # Display creation message
        hou.ui.displayMessage('{} node created!'.format(geometryName))
    else:
        # Display fail message
        hou.ui.displayMessage('{} already exists in the scene'.format(geometryName))

# Execute node creation 
createGeoNode(name)
```

### Create and setup UI
One of the possible workflows for building tools with UI in Houdini is creating interfaces in QT Designer (shipped with Python27) and importing *.ui files into your Python code where you will set up the functionality of UI elements.

Build a UI with a single button. Rename button object to `btn_create`.

### Run tool from the Shell