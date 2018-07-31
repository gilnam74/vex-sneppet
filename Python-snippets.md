# Introduction
Here you can find small code chunks to perform miscellaneous tasks in Houdini

### Get groups
```Python
import hou

node = hou.selectedNodes()[0]
groups = [g.name() for g in node.geometry().primGroups()]
print groups
```

### Expand Alembic
```Python
# Expand Alembic
# Rebuild single alembic SOP from multiple geometry nodes using Alembic hierarchy
# Select Alembic node, run script

# Naming convention
# Hirarchy in alembic: OBJECT (group)/PART_01, ..., PART_## (meshes)
# OBJECTS: <objectName>_<objectVariation> : bootle_A
# PARTS: <objectName>_<objectVariation>_<objectPart> : bootle_A_label

import hou

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
    
    
objectsMap = buildObjectsMap(listGroups)

def expandABC():
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
        # Get Alembic SOP
        ABC = hou.selectedNodes()[0]
        # Setup Alembic properties
        ABC.parm('loadmode').set(1) 
        ABC.parm('groupnames').set(4) 
        # Get Alembic container
        OBJ = ABC.parent()
        
        # Get all groups (PARTS) from alembic
        listGroups = [g.name() for g in ABC.geometry().primGroups()]
        
        # Expand Alembic
        expandABC()
        
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