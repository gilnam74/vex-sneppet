# VEX snippets
Here you will find isolated blocks of VEX code, each of them performs one certain small task.  

## Basic manipulations
Core one line stuff
#### Attributes
```C
// Get attribute value:  
vector pointPos = v@opinput0_P;
vector pointPos = point(0, "P", @ptnum);
vector pointPos = point("op:../geometryName", "P", @ptnum); 

// Create color attribute and set it`s value to red
addpointattrib(0, "Cd", {1,0,0});

// Create point attribute and set value
// Could be used to create and set point attributes in detail mode!
setpointattrib(0, "<attributeName>", <pointNumber>, <valutToSet>, "set");  

// Set attribute value:
v@vectorAttribute = {1, 2, 3};  
v@vectorAttribute = set(1, 2, @P.z);  
```
#### Modify input values
```c
f@attribute;
// Modify with fit range
@attribute = fit(@attribute, <currentMinValue>,<currentMaxValue>, <outMin>, <outMax>);
// Modify with a ramp
@attribute = chramp('Modify_Value',@attribute);
```

## Basic procedures
More complex solutions

#### Rotate with matrix
```C
// Create rotation matrix
matrix3 matrx = ident();
// Create angle control with UI
float angle = radians( chf('angle') );
// Define rotation axis
vector axis = {0, 1, 0};

//Rotate the matrix
rotate ( matrx, angle, axis); 

// Apply rotation: multiply position by matrix
@P *= matrx; 
```

#### Rotate with quaternion
```C
// Define rotation angle and axis
float angle = radians(chf('angle'));
vector axis = {0, 1, 0};

// Apply rotation
@P = qrotate(quaternion(angle, axis), @P);
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