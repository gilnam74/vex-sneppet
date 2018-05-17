Here you will find isolated blocks of VEX code, each of them performs one certain small task.  

## Basic manipulations
Core one line stuff
#### Datatypes
```C
// Integers
int myInteger = 1;
i@myInteger = 1;
// Floats
float myFloat = 4.14;
f@myFloat = 3.14;
// String
string myStiring = 'C:/cache/animation.abc';
s@myString = 'C:/cache/animation.abc';
// Arrays
string variations[] = {'A','B','C'};
s[]@variations = {'A','B','C'};

```
#### Get and set attributes
```C
// Get attribute value from first Wrangle input:  
vector pointPos = v@opinput0_P;
vector pointPos = point(0, "P", @ptnum);
// Get attribute value from scene geometry:  
vector pointPos = point("op:../geometryName", "P", @ptnum); 

// Create color attribute and set it`s value to red
addpointattrib(0, "Cd", {1,0,0});

// Create point attribute and set value
// Could be used to create and set point attributes in detail mode!
setpointattrib(0, "<attributeName>", <pointNumber>, <valutToSet>, "set");  

// Set attribute value:
f@pi = 3.1415;
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

##### Add point attribute in Detail mode
```c
addpoint(0, {0,0,0});
setpointattrib(0, 'myAttribute', 0, 'attributrValue', "set");
```

##### Create groups
```c
// Add points with X position > 1 to group "high"
if (@P.x > 1){
    i@group_high = 1
    // Alternative option:
    // setpointgroup(0, 'high', @ptnum, 1, 'set');
    }

// Short form    
i@group_high = @P.x > 1 ? 1:0;

// And even shorter
i@group_high = @P.x > 1;
```

##### Multiply distribution (make small smaller, big bigger)
```c
value = pow(value, 8.0);
```



## Basic procedures
More complex solutions
#### VEX strings
```C
// Build fileName_##.abc with variable
int version = 1;
string fileName = sprintf('fileName_%02d.abc', version);
// result: fileName_01.abc
```
#### Randomize file name
```C
// Get random file from sim_A_01.abc, sim_B_01.abc, sim_C_01.abc
string variations[] = {'A','B','C'};
int variationIndex = rint(fit(rand(@ptnum), 0, 1, 0, 2));
string path = sprintf('D:/PROJECTS/VEX/geo/sim_%s_01.abc', variations[variationIndex])
```

#### Fade grid Y deformation closer to border
```C
float objectSize = getbbox_max(0).x;
float dist = distance(0,@P);
float offset = chf('offset');
float fade = chramp('fade', fit(dist, 0, objectSize + offset, 0, 1));
@P.y *= fade;
``` 

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

#### Rotate with quaternion along XYZ axys
```C
// Setup angle control with UI
float angle_X = radians(chf('angle_X'));
float angle_Y = radians(chf('angle_Y'));
float angle_Z = radians(chf('angle_Z'));

// Apply rotation
vector rotations = set(angle_X,angle_Y,angle_Z);
@P = qrotate(quaternion(rotations), @P);
```

#### Rotate copies with quaternion multiply
```C
@N;
@up = {0,1,0};

@orient = quaternion(maketransform(@N,@up));
vector4 rotate_X = quaternion(radians(ch('Rotate_X')),{1,0,0});
vector4 rotate_Y = quaternion(radians(ch('Rotate_Y')),{0,1,0});
vector4 rotate_Z = quaternion(radians(ch('Rotate_Z')),{0,0,1});

@orient = qmultiply(@orient, rotate_X);
@orient = qmultiply(@orient, rotate_Y);
@orient = qmultiply(@orient, rotate_Z);
```


#### Spiral 
```C
float angle;
vector pos = {0,0,0};
int npoints = chi('number_of_points');
float step = radians(ch('sweep'))/npoints;

for (int n=0; n<npoints; n++) {  
    angle = step * n; // Or: angle += step;
    
    pos.x = cos(angle);
    pos.y = angle/10 ;
    pos.z = sin(angle);

    addpoint(0, pos);
}
```

#### Spiral grow
```C
float angle;
vector pos = {0,0,0}; 
int npoints = chi('number_of_points');
float step = radians(ch('sweep'))/npoints;

for (int n=0; n<npoints; n++) {
    angle = step * n;

    pos.x = sin(angle) * angle; 
    pos.y = angle;
    pos.z = cos(angle) * angle;

    addpoint(0, pos);
}
```
#### Phylotaxis
```C
int count = 400;
float bound = 10.0;
float tau = 6.28318530; // 2*$PI
float phi = (1+ sqrt(5))/2; // Golden ratio = 1.618
float golden_angle = (2 - phi)*tau; // In radians(*tau)
vector pos = {0,0,0};
float radius = 1.0;
float theta = 0;
int pt;


vector polar_to_cartesian(float theta; float radius){
    return set(cos(theta)*radius, 0, sin(theta)*radius);
}

for (int n=0; n<count; n++){
    radius = bound * pow(float(n)/float(count), ch('power'));
    theta += golden_angle;
    
    pos = polar_to_cartesian(theta, radius);

    // Create UP, pscale and N attr
    pt = addpoint(0, pos);
    setpointattrib(0, "pscale", pt, pow(radius,0.5));
    setpointattrib(0, "N", pt, normalize(-pos));
    setpointattrib(0, "up", pt, set(0,1,0));
}
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