# Python in Houdini
Houdini Python API exists as **HOM** - Houdini Object Model (the module name is hou: `import hou`).  

## Python integration basics
Python integration in Houdini:
- Windows > Python Shell - Python shell for small scripts/commands  
- Windows > Python Source Editor - Python editor which saves the code with *.hip file. Each time you open the file Python code in Source Editor will be executed.    
- In the parameters fields of any node (switch to Python from HScript).  
- Python node in SOP context
- In assets: create asset, right-click > Type properties > Scripts tab.
- Python Panel - to create PySide UI

To use Python Script in all scenes you need to save Python script to the Shell.

Examples below meant to be executed in **Python Source Editor**. 

##### Get node from the scene
```python
import hou
node = hou.node('/<nodePath>/<nodeName>') # By name
node = hou.selectedNodes()[0] # By selection
# Get node content
node.children()
```

##### Create node in the scene
To create any node wiyh Python you have to set parent node for that. You need to create Geometry node in OBJ context.
```Python
# Get scene root node
OBJ = hou.node('/obj/')
# Create Geometry node in scene root
geometry = OBJ.createNode('geo')
```

```python
import hou
# Create transform node inside geo1
geometry = hou.node('/obj/geo1')
xform = geometry.createNode('xform') 
xform.moveToGoodPosition() # Align new node

# Create new transform node linked to existing transform
xformNew= xform.createOutputNode('xform')
```


##### Get and set parameters
```python
import hou
node = hou.selectedNodes()[0]

# get translate X
node.parm('tx').eval() 
hou.parm('/obj/geo1/tx').eval()
hou.ch('/obj/geo1/tx')

# set translate XYZ
node.parmTuple('t').set([0,1,0])
hou.parm('/obj/geo1/tx').set(2)
```

##### Get Translate X keyframes of selected node
```Python
import hou
node = hou.selectedNodes()[0]

node.parm('tx').keyframes()
```

##### Run hscript command form Python
```Python
# Run Redshift IPR
hou.hscript('Redshift_openIPR')
```

##### Get all node parameters names
```Python
def getAllNodeParameters(node):
    # Return list of all parameters names for input node object 
    allParameters = [param.name()for param in node.parms()]
    return allParameters 
```

##### Connect nodes
```python
import hou
# Create transform nodes
xform_A = hou.node('/obj/geo1/transform1') 
xform_B = hou.node('/obj/geo1/transform2')
 # Connect transform_A to transform_B
xform_B.setInput(0, xform_A)

# Create merge
merge = node.createNode('merge')
# Connect xforms to a merge
merge.setNextInput(xform_A)
merge.setNextInput(xform_B)

# Get node inputs
merge.inputs()
# Get node outputs
merge.outputs()
```
##### Get groups
```Python
import hou

node = hou.selectedNodes()[0]
groups = [g.name() for g in node.geometry().primGroups()]
print groups
```
##### Builder workflow (shop context)
Create "Material Surface Builder" in SHOP context, dive inside.
```python
import hou
shader = hou.node('/shop/vopmaterial1/lambert1')
out = hou.node('/shop/vopmaterial1/surface_output')
out.setNamedInput('Cf', shader, 'clr') # Set connection by name
out.setNamedInput(0, shader, 0) # Set connection by parameter index

# List all inputs for node 'surface_output'
print out.inputNames()
```

##### Filter node.children() output
```python
import hou
selectedNode = hou.selectedNodes()

def extractVop(listOfChildrens):
    for node in listOfChildrens:
        if node.type().name() == 'vopsurface':
            return node

# return vopsurface nodes
vops = extractVop(selectedNode.children())
```

Same task with list comprehensions:
```python
import hou
selectedNode = hou.selectedNodes()

# return vopsurface nodes
vops = [node for node in selectedNode.children() if node.type().name() == 'vopsurface']
```

## PySide interfaces in Houdini
One of the possible workflows for building tools with UI in Houdini is creating interfaces in QT Designer (shipped with Python27) and importing *.ui files into your Python code where you will develop the tool functionality.

Create and save a UI file with QT Designer. It could be just a blank widget (but not Main Window). In Houdini run this code in Python Source Editor window:

```python
# Run *ui file in Houdini

import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class MyWidget(QtWidgets.QWidget):
    def __init__(self):
        super(MyWidget,self).__init__()
        ui_file = 'path/to/file.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)

win = MyWidget()
win.show()
```