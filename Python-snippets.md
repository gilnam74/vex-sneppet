# Introduction
Here you can find small code chunks to perform miscellaneous tasks in Houdini

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