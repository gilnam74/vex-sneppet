# Python in Houdini
Houdini Python API exists as **HOM** - Houdini Object Model (the module name is `hou`).  

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
node = hou.node('/<nodePath>/<nodeName>')
```

##### Create node in the scene
To create any node wiyh Python you have to set parent node for that.
```python
# Create transform node inside geo1
node = hou.node('/obj/geo1')
xform = node.createNode('xform') 
xform.moveToGoodPosition()
```


##### Get and set parameter for selected node
```python
node = hou.selectedNodes()[0]
node.parm('tx').eval() # get translate X
node.parm('tx').set(2) # set translate X
```