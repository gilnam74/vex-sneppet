# VEX snippets
Here you will find isolated blocks of VEX code, each of them performs one certain small task.  

#### Attributes
Here is the basics of attributes manipulations

```C
// Get attribute value:  
vector pointPos = v@opinput0_P;
vector pointPos = point(0, "P", @ptnum);
vector pointPos = point("op:../geometryName", "P", @ptnum); 

// Add color attribute and set it to red
addpointattrib(0, "Cd", {1,0,0});

// Create point attribute and set value
// Could be used to create and set point attributes in detail mode!
setpointattrib(0, "<attributeName>", <pointNumber>, <valutToSet>, "set");  

// Set attribute value:
v@vectorAttribute = {1, 2, 3};  
v@vectorAttribute = set(1, 2, @P.z);  
```

#### Create geometry from points array:
```c
float searchRadius = ch('searchRadius');
int nearpnts[] = nearpoints(0, @P, searchRadius);
foreach (int pnt;  nearpnts){
    if(pnt != @ptnum){
        int line = addprim(0, 'polyline');
        addvertex(0, line, @ptnum);
        addvertex(0, line, pnt );
        }
    } 
```