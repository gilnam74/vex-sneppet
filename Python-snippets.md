# Introduction
Here you can find small code chunks to perform miscellaneous tasks in Houdini.

You can explore the node parameters with Python Shel:
- Create any node and tweak its parameters
- Run Python Shell.  
- Type `node = `, drag node to Shell (you will get `hou.node('path/to/node')`), press enter.  
- Type `print node.asCode()`

## Snippets
##### Get Houdini environment variable
```python
import hou

# print current scene name
print hou.expandString("$HIPNAME")
```

##### Scene file operations
```python
import hou
sceneRoot = hou.node('/obj')

# Save current scene as file
hou.hipFile.save('C:/temp/myScene_001.hipnc')

# Export selected node to a file
sceneRoot.saveChildrenToFile(hou.selectedNodes(), [], 'C:/temp/nodes.hipnc')

# Import file to the scene
sceneRoot.loadChildrenFromFile('C:/temp/nodes.hipnc')
```
 
##### Get node from the scene
```python
import hou
node = hou.node('/<nodePath>/<nodeName>') # By name
node = hou.selectedNodes()[0] # By selection
# Get node content
node.children()
```

# Get node upstream connections
```Python
listParents = node.inputAncestors()
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
##### Delete node
```python
import hou
node = hou.node('/<nodePath>/<nodeName>')
node.destroy() # Delete node
```
##### Delete parameter expression (chennal, animation)
```python
import hou
node = hou.node('/<nodePath>/<nodeName>')
node.parm(<parameterName>).deleteAllKeyframes()
```

##### Copy node to another location
```python
import hou
node = hou.node('/<nodePath>/<nodeName>')
parent = hou.node('/<parentPath>/')
hou.copyNodesTo([node], parent)
```

##### Get and set parameters
```python
import hou
node = hou.selectedNodes()[0]

# get translate X
node.parm('tx').eval() 
hou.parm('/obj/geo1/tx').eval()
hou.ch('/obj/geo1/tx')

# Get string parameter without token evaluation
node = hou.node('/obj/geometry/fileCache')
print node.parm('file').eval() 
print node.parm('file').rawValue() 
# >> C:/temp/myFile.1.bgeo.sc
# >> $HIP/myFile.$F.bgeo.sc

# set translate XYZ
node.parmTuple('t').set([0,1,0])
hou.parm('/obj/geo1/tx').set(2)

# Set parameters for selected Remesh SOP
remesh = hou.selectedNodes()[0]
remesh.setParms({'group': 'myGroup', 'element_sizing1': 1, 'iterations': 2})
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
##### Run PySide UI
Create and save a UI file with QT Designer. It could be just a blank widget (but not Main Window). In Houdini run this code in Python Source Editor window:

```python
# Run *ui file in Houdini

import hou
from PySide2 import QtCore, QtUiTools, QtWidgets

class MyWidget(QtWidgets.QWidget):
    def __init__(self):
        super(MyWidget,self).__init__()
        ui_file = 'C:/path/to/file.ui'
        self.ui = QtUiTools.QUiLoader().load(ui_file, parentWidget=self)
        self.setParent(hou.ui.mainQtWindow(), QtCore.Qt.Window)

win = MyWidget()
win.show()
```
You should have your window opened. 

## Tools

### Expand Alembic
```Python
# Expand Alembic
# Recreate alembic hierarchy by object groups and names
# Select Alembic node, set 

# Naming convention
# Hirarchy in alembic: OBJECT (group)/PART_01, ..., PART_## (meshes)
# OBJECTS: <objectName>_<objectVariation> : bootle_A
# PARTS: <objectName>_<objectVariation>_<objectPart> : bootle_A_label

import hou
    
# Get Alembic SOP
ABC = hou.selectedNodes()[0]

def checkConditions():
    '''
    Check if environment conditions allows to run script without errors
    '''
    if not ABC:  # If user select anything
        print '>> Nothing selected! Select Alembic SOP!'
        return 0


def buildObjectsMap(listGroups):
    # Create object map dictionary: each object = key, list of parts = values
    
    objectsMap = {} # { OBJ: [PARTs] }
    
    for partNameFull in listGroups:
        items = partNameFull.split('_')
        object = '{0}_{1}'.format(items[0], items[1])
        part = items[2]
        
        if not object in objectsMap.keys():
            objectsMap[object] = [part]
        else:
            objectsMap[object].append(part)
    
    return objectsMap
        
def buildGroupsList(object, listParts):
    # Create string for BLAST SOP with list of groups for each object    

    groupsList = ''
    for part in listParts:
        name = '{}_{}'.format(object, part)
        groupsList += ' ' + name

    return groupsList
    
    

def expandABC(OBJ, objectsMap):
    # Recreate alembic hierarchy
    
    for object in sorted(objectsMap.keys()):             
        groupsList = buildGroupsList(object, objectsMap[object])
        blast = OBJ.createNode('blast')
        blast.setNextInput(ABC)
        blast.setName(object)
        blast.parm('group').set(groupsList)
        blast.parm('negate').set(1)
        blast.moveToGoodPosition()

def run():
    if checkConditions() != 0:    
        # Setup Alembic properties
        ABC.parm('loadmode').set(1) 
        ABC.parm('groupnames').set(4) 
        # Get Alembic container
        OBJ = ABC.parent()
        
        # Get all groups (PARTS) from alembic
        listGroups = [g.name() for g in ABC.geometry().primGroups()]
        
        # Build Objects Map
        objectsMap = buildObjectsMap(listGroups)
        # Expand Alembic
        expandABC(OBJ, objectsMap)
        
        print '>> EXPANDING DONE!'     
        
run()
```

### Import FBX into Houdini
```Python
import hou
import os 

dirFBX = 'P:/PROJECTS/NSI/PROD/3D/lib/ANIMATION/CHARACTERS/ROMA/FBX/' 
filesNamesFBX = [fileName for fileName in os.listdir(dirFBX) if os.path.isfile(os.path.join(dirFBX, fileName))]

for fileName in filesNamesFBX:
    fileFBX =  '{0}{1}'.format(dirFBX,fileName)
    if not hou.node('/obj/{}_fbx'.format(fileName.replace('.fbx',''))):
        hou.hipFile.importFBX(fileFBX) 
    else:
        print 'FBX {} EXISTS!'.format(fileName)
```

### Convert imported FBX to geometry 
```Python
# 256 Pipeline tools
# Convert FBX subnetwork to Geometry node
# Import FBX into Houdini, select FBX subnetwork, run script in Python Source Editor

import hou
# Get selected FBX container and scene root
FBX = hou.selectedNodes()
OBJ = hou.node('/obj/')

def checkConditions():
    '''
    Check if environment conditions allows to run script without errors
    '''
    if not FBX:  # If user select anything
        print '>> Nothing selected! Select FBX subnetwork!'
        return 0

def convert_FBX():
    '''
    Create Geometry node and import all FBX part inside
    '''
    # Create Geometry node to store FBX parts
    geometry = OBJ.createNode('geo', run_init_scripts = False)
    geometry.setName('GEO_{}'.format(FBX.name()))
    geometry.moveToGoodPosition()
    # Get all paerts inside FBX container
    geometry_FBX = [node for node in FBX.children() if node.type().name() == 'geo']
    
    # Create merge node for parts
    merge = geometry.createNode('merge')
    merge.setName('merge_parts')
    
    # Replicate FBX structure in Geometry node
    for geo in geometry_FBX: 
        # Create Object Merge node
        objectMerge = geometry.createNode('object_merge')
        objectMerge.setName(geo.name())
        # Set path to FBX part object
        objectMerge.parm('objpath1').set(geo.path())
        objectMerge.parm('xformtype').set(1)
        # Create Material node
        material = geometry.createNode('material')
        material.setName('MAT_{}'.format(geo.name()))
        # Link Material to Object Merge
        material.setNextInput(objectMerge)
        # Link part to Merge
        merge.setNextInput(material)
    
    # Set Merge Node flags to Render
    merge.setDisplayFlag(1)
    merge.setRenderFlag(1)
    # Layout geometry content in Nwtwork View 
    geometry.layoutChildren()

# Check if everything is fine and run script
if checkConditions() != 0:
    # Get FBX network
    FBX = FBX[0]
    # run conversion
    convert_FBX()
    print '>> CONVERSION DONE!'
```

### Create Material Stylesheet UI
```Python 
# Create Material Stylesheet parameter interface
# Select Geometry node, run script

import hou

# Get selected geometry node to create Stylesheet parameter
OBJ = hou.selectedNodes()[0]

# Define tags
dataTags = {'script_action_icon':'DATATYPES_stylesheet',
            'script_action_help':'Open in Material Style Sheet editor.',
            'spare_category':'Shaders',
            'script_action':"import toolutils\np = toolutils.dataTree('Material Style Sheets')\np.setCurrentPath(kwargs['node'].path() + '/Style Sheet Parameter')",
            'editor':'1'}

# Create parameter interface
group = OBJ.parmTemplateGroup()
folder = hou.FolderParmTemplate('folder', 'Shaders')
folder.addParmTemplate(hou.StringParmTemplate('shop_materialstylesheet', 'Material Style Sheet', 1, tags = dataTags))
group.append(folder)
OBJ.setParmTemplateGroup(group)
```

### Flatten curve
```Python
# Flatten curve: set Y coord = 0
coordList_SRC = '39.1665,0.362686,22.4173 55.3542,0.365759,10.9339'
coords = coordList_SRC.split(' ')

coordList_RES = ''
for xyz in coords:
    listXYZ_SRC = xyz.split(',')
    listXYZ_SRC = '{0},0.0,{1} '.format(listXYZ_SRC[0], listXYZ_SRC[2])
    coordList_RES += listXYZ_SRC
 
print coordList_RES
```