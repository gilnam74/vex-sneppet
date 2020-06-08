This is the reference for VEX development. Here you can find VEX code snippets that could be a good foundation to build your own tools.

Walkthrough the [VEX for artists](vex-for-artists) tutorial if you don`t have a general understanding of how VEX works and how it can be used to make your life easier (or, optionally, turn it into complete disaster after stepping into this rabbit hole).

These and other examples you can find in [VEX snippets hip file](../blob/master/src/hips/VEX_snippets_001.hiplc). 

[Snippets](#snippets) | [VEX expressions](#vex-expressions) | [Tools](#tools) | [Algorithms](#algorithms)

## Snippets
The VEX 101 and the most low-level solution blocks.

#### Datatypes
```C
// Integers
int myInteger = 1;
i@myInteger = 1;

// Floats
float myFloat = 4.14;
f@myFloat = 3.14;

// Strings
string myStiring = 'C:/cache/animation.abc';
s@myString = 'C:/cache/animation.abc';

// Arrays
string variations[] = {'A','B','C'};
string variables[] = array(variable_A, variable_B, variable_C);
s[]@variations = {'A','B','C'};
```

#### Data type convertion
```C
// interger >> string
int number = 123;
string text = itoa(number);

// string >> integer
string text = '123';
int number = atoi(text);
```

#### Strings
```C
// Print strings
printf('Hello, World!');  
// Result: Hello, World!

// Print string variable
string text = 'Hello, World!';
printf('The text is: %s \n', text); 
// Result: The text is: Hello, World!

// Slice strings
// string[start:stop]  >> Use <start> through <stop>-1
string text = '123456';
text[:];     // Result: '123456'
text[:-1];   // Result: '12345'
text[1:];    // Result: '23456'
text[1:-1];  // Result: '2345'
text[-1];    // Result: '6'
text[0];     // Result: '1'

// Concatenate (join) strings
string node = 'SOP';
string value = '256';
string output = sprintf('%s%s%s', node, ' = ', value);
printf('%s', output);
// Result: 'SOP = 256'

// Reverse
string text = 'ABCD''
reverse(text);
// Result: 'DCBA'
```

#### Arrays
```C
// Accessing, sorting, reversing elements
int numbers[] = {5, 4, 3, 2, 1};
printf(' %d\n', numbers[0]);                // Result: 5
printf(' %d\n', numbers[-1]);               // Result: 1
printf(' %d\n', numbers[2:]);               // Result: {3, 2, 1}
printf(' %d\n', sort(numbers));             // Result: {1, 2, 3, 4, 5}
printf(' %d\n', reverse(sort(numbers)));    // Result: {5, 4, 3, 2, 1}

// Split string with a space to array of strings
string numbres = '1 2 3 4 5 6';
string array[] = split(numbres, ' ');
printf('%s', array);
// Result: {1, 2, 3, 4, 5, 6}

// Split integer into array of integers via strings
int int_number = 312654;
string string_number = itoa(int_number);
int int_numbers[];

for(int n=0; n<len(string_number); n++){  
        //int_numbers[n] = atof(string_numbers[n]);
        int_numbers[n] = atoi(string_number[n]);
        }

printf('%s', int_numbers);
// Result: {3, 1, 2, 6, 5, 4}
printf('%s', sort(int_numbers));
// Result: {1, 2, 3, 4, 5, 6}
printf('%s', reverse(sort(int_numbers)));
// Result: {6, 5, 4, 3, 2, 1}
```

#### Get and set attribute values
```C
// Get attribute value from first Wrangle input:  
vector point_pos = v@opinput0_P;
vector point_pos = point(0, "P", @ptnum);

// Get attribute value from scene geometry:  
vector point_pos = point("op:../geometry_name", "P", @ptnum); 

// Get primitive attribute in point mode
primattrib(0, "attribute_name", @ptnum, 0);

// Create color attribute and set it`s value to red
addpointattrib(0, "Cd", {1,0,0});

// Create point attribute and set value
// Could be used to create and set point attributes in detail mode
setpointattrib(0, "<attribute_name>", <point_number>, <value>, "set");  

// Set attribute value:
f@pi = 3.1415;
v@vector_a = {1, 2, 3};  
v@vector_b = set(1, 2, @P.z);  
```

#### Modify input values
```c
float input;
// Modify with fit range
input = fit(input, <currentMinValue>,<currentMaxValue>, <outMin>, <outMax>);
// Modify with a ramp
input = chramp('Modify_Value', input);
```

#### Get points and primitives
```c
// Run over points
int points[] = expandpointgroup(0, "!*");
int primitives[] = expandprimgroup(0, "!*");
// Run over primitives
int points[] = primpoints(0, @primnum); 
```

#### Add point attribute in Detail mode
```c
addpoint(0, {0,0,0});
setpointattrib(0, 'myAttribute', 0, 'attributrValue', "set");
```
#### Debug VEX with print
```c
// Basic print
printf('Hello, World');

// Print data
printf('The string is %s', 'Eve');         // Result: The string is Eve
printf('The integer is %d', 256);          // Result: The integer is 256
printf('The float is %f', 3.14156);        // Result: The float is 3.14156
printf('The the float is %.2f', 3.14156);  // Result: The float is 3.14

// Get all primitives
int primitives[] = expandprimgroup(0, "*");

foreach (int currentPrim; primitives){   
        // print primitive number
        printf('Prim %s \n', currentPrim);
        }
```

#### Create groups
```c
// Add points with X position > 1 to group "high"
if (@P.x > 1){
    setpointgroup(0, 'high', @ptnum, 1, 'set');
    // Alternative: i@group_high = 1
    }

// To highlight VEX group in the viewport enter group name 
// in the <Output Selection Group> field
// in <Bindings> tab of the Wrangler node.
```

#### Loops
```c
// Create OPEN shape 

// Create LINE primitive
int primitive = addprim(0, 'polyline'); 
// Calculate total number of points
int numberOfPoints = @numpt;

// Create a vertex for each point in primitive
for (int n=0; n<numberOfPoints; n++){
    addvertex(0, primitive, n);  
    }
```

```C
// Create CLOSED shape

// Create POLYGON primitive
int primitive = addprim(0, 'poly');
// Store all points in array
int allPoints[] =  expandpointgroup(0, "!*");

// Create a vertex for each point in primitive
foreach (int currentPoint; allPoints){     
        addvertex(0, primitive, currentPoint);
        }
```

#### Conditions
```c
// Scale 10 times first and last points
if ((@ptnum == 0) || (@ptnum == (@numpt-1))) f@pscale = 10; 
else f@pscale = 1;
```

```C
// Scale 10 times first and last points, short form    
f@pscale = (@ptnum == 0) || @ptnum ==(@numpt-1) ? 10 : 1;
```

#### Vectors
```c
// Find a vector between two points A and B (coords B - coords A)
vector vector_A = normalize(point(0, "P", <ptnum_B>));
vector vector_B = normalize(point(0, "P", <ptnum_A>));
vector vector_AB = vector_A-vector_B;

// Build tangent normals
vector vector_A = normalize(point(0, "P", @ptnum));
vector vector_B = normalize(point(0, "P", @ptnum + 1));
@N = vector_A-vector_B; 

// Get angle between 2 vectors in radians
float angle = acos(dot(normalize(vector_A), normalize(vector_B)));
```

### Transformation matrix
```c
// Get matrix from scene object
matrix matrx = optransform('obj/geometry_01');
// Apply object transforms to a points
@P *= matrx;
```


## VEX expressions
Using VEX in the parameter interface of Houdini nodes. See [documentation](http://www.sidefx.com/docs/houdini/expressions/index.html)

#### Get Attributes
```c
detail("../nodeName/", 'attributeName', 0)
point("../nodeName/",@ptnum, 'attributeName',0)
```
#### Every N frame
```c
if(($F % N == 0),$F,0)
// Hscript version: floor($F/N)*N
```

#### Select corner points
```c
# create Groupexpreesion SOP
neighbourcount(0, @ptnum) == 2
```

## Tools
In this section, there are a bit more sophisticated VEX solutions. Each solves some particular task and can be considered as a custom tool.

### Flatten mesh by UVs
```c
// Plave points as UVs in 3d
v@rest = @P;
@P = vertex(0, "uv", pointvertex(0, @ptnum));

// Return them back
@P = v@rest 
```

### Remap random from 0:1 to -1:1
```c
float random = rand(@ptnum)*2-1;
```

### Bend (curl) curves (hairs)
```c
// Primitive wrangle
int points[] = primpoints(0, @primnum); 

matrix3 matrx = ident();
float angle = radians( chf('angle') );
vector axis = {1, 0, 0};

vector init_pos = point(0, "P", points[0]);
vector prev_pos = init_pos;

for (int n=0; n<len(points); n++){
    vector curr_pos = point(0, "P", points[n]);
    rotate(matrx, angle, axis);
    
    // init_pos *= 0.01; // spiral
    vector new_pos = (curr_pos - init_pos)*matrx + prev_pos;
    init_pos = curr_pos;
    prev_pos = new_pos;
    
    setpointattrib(0, "P", points[n], new_pos); 
    }

```


### Create UVs on curves (hairs) and paint with ramp and random color
```c
// For input cluster of curves
// Set uv attribute from 0 at a root, to the 1 at a tip
f@uv = float(vertexprimindex(0, @ptnum))/(@numvtx-1);

// Paint curve and correct with ramp
@Cd = chramp('Value',@uv); 
 
// Add random 10% of red curves
if(rand(@primnum) > 0.9){
    @Cd={1,0,0};
    }
```  

### Stick points to animated geometry
Create TimeShift SOP after animated geo, Scatter SOP and Attribute Wrangler. Connect scatter, timeShift, animated geo to inputs 0, 1 and 2 of the wrangle.
```c
int prim;
vector uv;

// What prim the scatterd point is close to, and position of this prim in uv space
xyzdist(1, @P, prim, uv);
// Set scattered point position 
@P = primuv(2, "P", prim, uv);
```

#### Move an object to the origin and return back
Create wrangle to move object to the origin
```c
// Get center of the oject bounding box (centroid)
vector min = {0, 0, 0};
vector max = {0, 0, 0};
getpointbbox(0, min, max);
vector centroid = (max + min)/2.0;

// Build and apply transformation matrix
vector translate = centroid;
vector rotate = {0,0,0};
vector scale = {1,1,1};
matrix xform = invert(maketransform(0, 0, translate, rotate, scale));
@P *= xform;

// Store transformation matrix in attribute
4@xform_matrix = xform;
```

Create the second wrangle to return it to the original position
```c
@P *= invert(4@xform_matrix);
```

#### Use Noise function
```c
// Visualise nose as Black and White values
// Delete black and white points separatly

// Default non zero values for 10X10 grid:
// Noise_size = 1
// Noise_threshold = 0.5

// Make geometry white
@Cd = {1, 1, 1};

// Setup noise
float noseValues = noise(@P*(1/chf('Noise_Size')) + chf('Noise_Offset'));

// Paint-delete points with noise
if(noseValues > chf('Noise_Threshold')){
    @Cd = 0;
    if(rand(@ptnum) < ch('delete_black')){
        if(chi('del') == 0){
            @Cd = {1,0,0};
            }
        else{
            removepoint(0,@ptnum);
            }
        }
    }

if(noseValues < chf('Noise_Threshold')){
    if ( rand(@ptnum) < ch('delete_white') ) {       
        if(chi('del') == 0){
            @Cd = {1,0,0};
            }
        else{
            removepoint(0,@ptnum);
            }
        }
    }
```

#### Flatten surface bottom
```c
float min = ch("flatten_disrtance") + getbbox_min(0).y;
float max = getbbox_max(0).y;
float Y = clamp(@P.y, min, max);

@P = set(@P.x, Y, @P.z);

```

#### Multiply distribution (make small smaller, big bigger)
```c
value = pow(value, 8.0);
```

#### Noise the points
```c
// Define UI controls
float noise = chf('Noise_Power');
float freq = chf('Noise_Frequency');
// Create noise
vector noiseXYZ = noise(@P*freq);
// Apply noise to a point position
v@ns = fit(noiseXYZ, 0,1, -1, 1)*noise;
@P.x  += @ns.x;
@P.z  += @ns.z;
```
#### Select mesh border points
```c
// Get number of connectet points
int nbPts = neighbourcount(0,@ptnum);
// Create "border" group with border points
i@group_border = nbPts == 3 | nbPts == 2; 
```
#### Shape Polywire with ramp for combined curves
```c
// Create Primitive Wrangle before polywire, use @width as Wire Radius
// Get array of points in each curve (primitive)
i[]@primPts = primpoints(0, @primnum);

// For each point in current curve
foreach (int i; int currentPoint; @primPts){
    float ramp_index = fit(i, 0, len(@primPts)-1, 0,1);
    f@widthPrim = chramp("shape", ramp_index)/20;
    setpointattrib(0, "width", currentPoint, @widthPrim, "set"); 
    }
```

#### VEX strings
```C
// Build fileName_##.abc with variable
int version = 1;
string fileName = sprintf('fileName_%02d.abc', version);
// result: fileName_01.abc
```

#### Find closest points
```c
float maxdist = 0.8;
int maxpoints = 10;

int closept[] = pcfind(0, 'P', @P, maxdist, maxpoints);
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
float objectSize = (getbbox_max(0).x + getbbox_max(0).z)/2;
float dist = distance(0,@P);
float offset = chf('offset');
float fade = chramp('fade', fit(dist, 0, objectSize + offset, 0, 1));
@P.y *= fade;
``` 

#### Fade noise on curves with ramp
```c
// Requires uvtexture SOP in "Pts and Columns" mode before this wrangle

// Define UI controls
float remap_uv = chramp('remap_uv', @uv.x);
float power = chf('Noise_Power');
float freq = chf('Noise_Frequency');

// Create noise
vector noiseXYZ = noise(@P*freq);
// Modify noise values
vector displace = fit(noiseXYZ, 0,1, -1, 1)*power*remap_uv;
// Apply modified noise to a points position
@P += displace;
// Visualize fade ramp on curve
@Cd = remap_uv;
```

#### Rotate GEO with matrix along Y axis
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

#### Adjust pivot point of rotation matrix
```C
matrix3 matrx = ident();
float angle = radians(36);
vector axis = {1, 0, 0};
vector pivot = {0, 2.56, 0};

rotate ( matrx, angle, axis); 
@P = (@P - pivot) * matrx + pivot; 
```

#### Rotate GEO with quaternion along XYZ axys
```C
// Setup angle control with UI
float angle_X = radians(chf('angle_X'));
float angle_Y = radians(chf('angle_Y'));
float angle_Z = radians(chf('angle_Z'));

// Apply rotation
vector rotations = set(angle_X,angle_Y,angle_Z);
@P = qrotate(quaternion(rotations), @P);
```

#### Rotate Y COPIES with quaternion multiply
```C
@N;
@up = {0,1,0};

@orient = quaternion(maketransform(@N,@up));
vector4 rotate_Y = quaternion(radians(ch('Rotate_Y')),{0,1,0});
@orient = qmultiply(@orient, rotate_Y);
```

#### Randomize copies
```c
// Define orientation vectors
@N;
@up = {0,1,0};

// Define random position values
float randPos_X = fit01(rand(@ptnum), -ch('Translate_X'), ch('Translate_X'));
float randPos_Y = fit01(rand(@ptnum), -ch('Translate_Y'), ch('Translate_Y'));
float randPos_Z = fit01(rand(@ptnum), -ch('Translate_Z'), ch('Translate_Z'));
vector randPos = set(randPos_X, randPos_Y, randPos_Z);

// Define random rotation values
float randRot_X = fit01(rand(@ptnum), -ch('Rotate_X'), ch('Rotate_X'));
float randRot_Y = fit01(rand(@ptnum), -ch('Rotate_Y'), ch('Rotate_Y'));
float randRot_Z = fit01(rand(@ptnum), -ch('Rotate_Z'), ch('Rotate_Z'));

// Apply random positions
@P += randPos; 

// Apply random rotations
@orient = quaternion(maketransform(@N,@up));
vector4 rotate_X = quaternion(radians(randRot_X),{1,0,0});
vector4 rotate_Y = quaternion(radians(randRot_Y),{0,1,0});
vector4 rotate_Z = quaternion(radians(randRot_Z),{0,0,1});
@orient = qmultiply(@orient, rotate_X);
@orient = qmultiply(@orient, rotate_Y);
@orient = qmultiply(@orient, rotate_Z);

// Apply random scale
@scale = fit01(rand(@ptnum), chf('Scale_MIN'), chf('Scale_MAX'));
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

## Algorithms

#### Reverse array
```C
int int_numbers[] = {1,2,3,4,5,6};
int rversed[];

for(int i=0; i<len(int_numbers)/2; i++){

    int number_from_start = int_numbers[i];
    int index_from_end = len(int_numbers)-i-1;

    rversed[i] = int_numbers[index_from_end];
    rversed[index_from_end] = number_from_start;
    }

printf('%s', rversed);
// Result: {6, 5, 4, 3, 2, 1}
```

#### Choise sorting
```C
int numbers[] = array(0,4,3,2,1);

for(int i=0; i<len(numbers)-1; i++){        
    for(int n=i+1; n<len(numbers); n++){        
        if(numbers[i]>numbers[n]){

            int swap = numbers[i];
            numbers[i] = numbers[n];
            numbers[n] = swap;            
            }
        }   
    }
    
printf('Array = %s\n', numbers);
// Result: Array  = {0, 1, 2, 3, 4}
```

#### Buble sorting
```C
int numbers[] = array(0,4,3,2,1);

for(int i=1; i<len(numbers); i++){        
    for(int n=0; n<len(numbers)-i; n++){          
        if(numbers[n]>numbers[n+1]){
            int swap = numbers[n];
            numbers[n] = numbers[n+1];
            numbers[n+1] = swap;
            }  
        }   
    }
    
printf('Array = %s\n', numbers);
// Result: Array  = {0, 1, 2, 3, 4}
```
