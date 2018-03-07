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
To create any node wiyh Python you have to set parent node for that. You need to create Geometry node in OBJ context.
```python
import hou
# Create transform node inside geo1
node = hou.node('/obj/geo1')
xform = node.createNode('xform') 
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