# Introduction
Here you can find small code chunks to perform miscellaneous tasks in Houdini

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

### Convert animated FBX to file caches and apply materials
```Python
# 256 Pipeline tools
# Convert FBX subnetwork to Geometry node
# Import FBX into Houdini, select FBX subnetwork, run script in Python Source Editor

import hou
# Get selected FBX container and scene root
FBX = hou.selectedNodes()
OBJ = hou.node('/obj/')
fileVersion = '001'

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
        objectMerge.setName('GEO_{}'.format(geo.name()))
        # Set path to FBX part object
        objectMerge.parm('objpath1').set(geo.path())
        objectMerge.parm('xformtype').set(1)
        # Create Group node
        group = geometry.createNode('groupcreate')
        group.setName('{}'.format(geo.name()))
        group.parm('groupname').set('$OS')
        # Link Group to Object Merge
        group.setNextInput(objectMerge)
        # Link part to Merge
        merge.setNextInput(group)
    
    # Create Material Node
    mat = geometry.createNode('material') 
    mat.setNextInput(merge)
    mat.setName('MATERIAL')
    # Hardcode ROMA material asignment
    mat.parm('num_materials').set('3')
    mat.parm('group1').set('ROMA_EYE_L_OUT ROMA_EYE_R_OUT')
    mat.parm('shop_materialpath1').set('/obj/MATERIALS/GENERAL/GEN_EYE')
    mat.parm('group2').set('ROMA_EYE_L_BLACK ROMA_EYE_R_BLACK')
    mat.parm('shop_materialpath2').set('/obj/MATERIALS/GENERAL/GEN_PUPIL')
    mat.parm('group3').set('ROMA_ARMS ROMA_BEARD ROMA_HEAD ROMA_IRON ROMA_PENS ROMA_SHOES ROMA_TSHIRT')
    mat.parm('shop_materialpath3').set('/obj/MATERIALS/GENERAL/GEN_BASE')
    
    
    
    # Create groupdelete node
    gdel = geometry.createNode('groupdelete') 
    gdel.setNextInput(mat)
    gdel.setName('CLEAN')
    gdel.parm('group1').set('*')
    
    # Create FileCache    
    fileName = FBX.name().split('_')[0]
    cache = geometry.createNode('filecache')    
    cache.parm('file').set('$JOB/lib/ANIMATION/CHARACTERS/ROMA/GEO/{0}/{1}/{0}_{1}.$F.bgeo.sc'.format(fileName, fileVersion))
    cache.setNextInput(gdel)
    cache.setName('CACHE_ANIM')
    
    # Set File CacheNode flags to Render
    cache.setDisplayFlag(1)
    cache.setRenderFlag(1)
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