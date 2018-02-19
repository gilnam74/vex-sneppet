# Python in Houdini
Houdini Python API exists as **HOM** - Houdini Object Model (the module name is `hou`).  
`hou.node('/obj/geo1')`

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