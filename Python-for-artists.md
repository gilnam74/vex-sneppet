# Introduction
I hope nobody doubts that the ability to write even simple scripts it's a huge benefit for a CG artist. Why then it's not that common for artists to be able to code? Probably there is only one reason besides laziness: people thinking that coding is hard and requires a special gift. But this is not the case, anybody can build programs that will make their life way easier.

Follow this tutorial and you will get the knowledge on how to build your own working tools!

When dealing with a programming we should decide which language we would use to achieve our goals first. And choosing the proper language sometimes is a really sophisticated task required a huge experience in computer science. Fortunately for CG artist, this choice has been already made by the developer of your favorite software. Lucky users of Maya, Nuke, Houdini and a lot of other major DCC (hello, Adobe!) support Python. And this is super awesome great because Python is very easy to learn and use.

[Python in Houdini basics](#python-integration-basics)  |  [Creating a first custom tool](#creating-a-first-custom-tool)

## Python integration basics
You don't need to do any extra step to start writing Python code in Houdini, it is already there.

Python integrated into Houdini in several ways:
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
>>>> The fruits are Apple, Banana, Kiwi!
```
 
Now lets wrap a part of our program to a [function](Programming-basics#functions) and run this function:

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
Here we change the initial name of the variable `geometryName` to `name` to avoid confusion on how this variable is being passed through the code. If you remain the old name 'geometryName' the script will work the same. 

The last step in organizing is to separate two logical parts of our script: geometry node creation and checking if the node exists:

```python
import hou

# Define geometry node name
name = 'MY_GEO'

def checkExisting(geometryName):
    # Check if "MY_GEO" exists
    if hou.node('/obj/{}'.format(geometryName)):
        # Display fail message
        hou.ui.displayMessage('{} already exists in the scene'.format(geometryName))
        return True
    

def createGeoNode(geometryName):
    # Get scene root node
    sceneRoot = hou.node('/obj/')
    # Create empty geometry node in scene root
    geometry = sceneRoot.createNode('geo', run_init_scripts=False)
    # Set geometry node name
    geometry.setName(geometryName)
    # Display creation message
    hou.ui.displayMessage('{} node created!'.format(geometryName))

# Execute node creation 
if checkExisting(name) != True:
    createGeoNode(name)
```

Here we have created `checkExisting()` function which checks if a node with input name (`name`) exists in the scene. If so, it raises the message window and returns zero (if not, it returns `None`). Later we run this function and check if it does not return zero (geometry not exists), we create a geometry node. Same functionality but a bit more sophisticated structure which is currently may seem over complicated but its more flexible for the future extensions. For example, if you will need to check more conditions later in addition to an existing name (if the user has select anything we need, if the selection is correct, etc) it will be more easy to implement and the code would be cleaner if we would try to fit everything in one createGeoNode() function.

### Create and setup UI
One of the possible workflows for [building tools with UI](python-snippets#run-pyside-ui) in Houdini is creating interfaces in QT Designer (shipped with Python27) and importing *.ui files into your Python code where you will set up the functionality of UI elements. 

In PySide, any UI element is called a **widget**. A window is a widget, a button is a widget too. If you have a button in the window this means that button is parented to a window. There are a lot of widgets in PySide to solve a variety of tasks, you can examine the whole list in **Widget Box** panel in QT Designer. For our GeoCreator tool will use 3 widgets:
- **QWidget** to create the GeoCreator window (a window will contain all UI we need for the tool), 
- **QPushButton** to create a button (it will run the program that we created earlier),
- **QLineEdit** to get a text from the user (to set the name of created geometry).

To get QT Designer you need to [install Python 2.7 and PySide](pipeline-tutorials#requirments-and-installation) on your system. QT designer will be located at `C:/Python27/Lib/site-packages/PySide/designer.exe` 

To build our UI:
- Run QT Designer and create a new widget: File > New > Widget. You will have an empty window widget. You can resize it and modify some properties in Object Inspector (vertical panel located on the right side). I change `ObjectName` to "GeoCreator" and `Window Title` to "Create Geometry"
- From Widget Box (vertical panel with a list of QT widgets, located on the left side) drag into our window:
    * Line Edit (Input Widgets section), change "ObjectName" parameter to `lin_name`, "text" to `MY_GEO`, 
    * Push Button (Buttons section), change "ObjectName" parameter to `btn_create`, "text" to `Create Geometry`, "minimumSize > height" = 35
- Select Window widget (either click on empty space in the window widget or click on GeoCreator in object inspector) and press "Lay Out Vertically" button on the toolbar.
- Save the file somewhere, for example `'C:/temp/uiGeoCreator.ui'`. Here you can download [uiGeoCreator.ui](../blob/master/hips/uiGeoCreator.ui)

[![](https://c2.staticflickr.com/2/1962/44356680464_2a3ba7d302_o.gif)](https://c2.staticflickr.com/2/1962/44356680464_2a3ba7d302_o.gif)

Now clean all code in Python Source editor, go to [Run PySide UI](python-snippets#run-pyside-ui) section of Python Snippets page, copy paste the code to Python Source editor and change the path to UI file:
```python
import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class GeoCreator(QtWidgets.QWidget):
    def __init__(self):
        super(GeoCreator,self).__init__()
        ui_file = 'C:/temp/uiGeoCreator.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)

win = GeoCreator()
win.show()
```
Run this code and you should get your UI in Houdini!
[![](https://c2.staticflickr.com/2/1955/43262621080_9542664e1b_o.gif)](https://c2.staticflickr.com/2/1955/43262621080_9542664e1b_o.gif)

This is a minimum amount of code to run UI file in Houdini, you can always use it as a starting point while developing your tools. Don't worry it contains `class` (if you have no idea what is it) or looks to scarry it would be not that hard to work with. You may take some Python tutorials about **Object-Oriented Programming** and **classes** but even without understanding those concepts you will be able to move forward and even build your own basic stuff.

Classes are the small programs, just like functions. Imagine classes as containers for basic functions. Our class "GeoCreator" contains one function `__init__()`, where we import UI file in code and setup how all widgets will work.

Any function declaration inside the class should contain `self` argument. When you define a function without a class:
```python
def helloWorld():
    print 'Hello, World!'
```
When you define a function inside the class:
```python
def helloWorld(self):
    print 'Hello, World!'
```
If you have a class named "GeoCreator" you will run this program same way as a usual function: `GeoCreator()`. If you assign the result of class execution to a variable (`win = GeoCreator()`) you will create an instance of a class — object `win`. Same thing we did when create a Houdini root scene object (`sceneRoot = hou.node('/obj/')`). Then you can apply a specific command to this objects, e.g. to display UI in Houdini we use `show()` command: `win.show()`. 

And you need PySide libraries in your program to make UI widgets works, so we import them at the beginning: `from PySide2 import QtCore, QtUiTools, QtWidgets`. This basic template will remain the same with any other tool you will want to create, you will need just to build actual functionality on top of that. 

Next, we will bring back the functional part of our code we develop previously and link everything with UI elements.

Let's discover how to work with UI widgets we have. We want something happened when the user presses the button, so we need to create a function in the class which will be executed on button click event. Let this function print "Hello, World!" for now:

```python
import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class GeoCreator(QtWidgets.QWidget):
    def __init__(self):
        super(GeoCreator,self).__init__()
        ui_file = 'C:/temp/uiGeoCreator.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)
        
    def buttonClicked(self):
        print 'Hello, World'

win = GeoCreator()
win.show()
```
If you run it now the button widget would not work, we need to link the event of particular button click with a corresponding function. We will setup all widgets in `__init__` function:

```python
import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class GeoCreator(QtWidgets.QWidget):
    def __init__(self):
        super(GeoCreator,self).__init__()
        ui_file = 'C:/temp/uiGeoCreator.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)
        
        # Setup "Create Geometry" button
        self.ui.btn_create.clicked.connect(self.buttonClicked)
        
    def buttonClicked(self):
        print 'Hello, World'

win = GeoCreator()
win.show()
```
Run the code, press "Create Geometry" button and you will get "Hello, World!" printed in Houdini Console. We link `buttonClicked()` function to our button widget with this line: `self.ui.btn_create.clicked.connect(self.buttonClicked)`, where `self.ui` is a UI object we created in QT Designer and imported into the code (window with button and text field), `btn_create` is a button widget which launch `buttonClicked` function (`connect(self.buttonClicked)`) and event which will run this function is a button click (`clicked`).

Now let's grab the text from the text filed (QLineEdit widget named lin_name) and print it instead of "Hello, World!". You can do it in `__init__` function via variable and send this variable value to `buttonClicked` function:

```python
import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class GeoCreator(QtWidgets.QWidget):
    def __init__(self):
        super(GeoCreator,self).__init__()
        ui_file = 'C:/temp/uiGeoCreator.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)
        
        # Get name from user
        customName = self.ui.lin_name.text()
        # Setup "Create Geometry" button
        self.ui.btn_create.clicked.connect(lambda: self.buttonClicked(customName))
        
    def buttonClicked(self, name):
        print name

win = GeoCreator()
win.show()
```
Where we take text from ui (`lin_name` widget) and put it in `customName` variable with `self.ui.lin_name.text()` and send this text to `buttonClicked` function `lambda: self.buttonClicked(customName)`

### Run tool from the Shell