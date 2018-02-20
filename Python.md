# Python in Houdini
Houdini Python API exists as **HOM** - Houdini Object Model (the module name is hou: `import hou`).  

Python integration in Houdini:
- Windows > Python Shell - Python shell for small scripts/commands  
- Windows > Python Source Editor - Python editor which saves the code with *.hip file.  
- In the parameters fields of any node (switch to Python from HScript).  
- Python node in SOP context
- In assets: create asset, right-click > Type properties > Scripts tab.
- Python Panel - to create PySide UI

To use Python Script in all scenes you need to save Python script to the Shell.

##### Get node from the scene
```python
import hou
node = hou.node('/<nodePath>/<nodeName>') # By name
node = hou.selectedNodes() # By selection
# Get node content
node.children()
```

##### Create node in the scene
To create any node wiyh Python you have to set parent node for that.
```python
import hou
# Create transform node inside geo1
node = hou.node('/obj/geo1')
xform = node.createNode('xform') 
xform.moveToGoodPosition() 
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

##### Connect nodes
```python
import hou
xform_A = hou.node('/obj/geo1/transform1')
xform_B = hou.node('/obj/geo1/transform2')
xform_B.setInput(0, xform_A)
```