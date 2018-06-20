# Introduction
For those who tried (or afraid to start) to learn VEX but fail and stop because it was too hard. Here you will learn VEX and some Math beginning from the basics with a clear detailed explanation. You need elementary knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops then you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

To understand **applied Math** in 3D using VEX in Houdini our mission here.

##### Create a wrangle in a fresh Houdini scene
- Press TAB in Node View, type `geometry`, hit ENTER
- Dive inside "Geometry" node, delete "File" node
- Press TAB, type `aw`, hit ENTER to create "Attribute Wrangle" node

If you need to **CREATE** data with Attribute Wrangle: switch **"Run Over"** parameter to **"Detail"**. If you will feed some data to Attribute Wrangle (connect any node to input) leave Run Over parameter at default Point state.

Article structure:  
- [VEX orientation](#vex-orientation)  
[point](#create-a-point) | [line](#create-a-line) | [circle](#create-a-circle)  
- [VEX first steps](#vex-first-steps)  
[sine](#sine)  |  [noise](#noise)
- [VEX basics](#vex-basics)

All exercises from this chapter you can find in [VEX snippets hip file](../blob/master/hips/VEX_snippets_004.hipnc).

# VEX orientation
The goal here is to start with the very simple and basic tasks keeping the amount of code minimal and gradually, step by step increase complexity of exercises. If you will really understand the basics it would be easy to develop extra functionality for the staring code.

## Attribute Wrangle
Imagine **Attribute Wrangle** as a [loop](Programming-basics#loops) node, for example Attribute Wrangle run over Points:

```c
for (every input point){
    do everything from VEX expression window;
    }
``` 
[![](https://c2.staticflickr.com/2/1734/42744305102_f3e25b516f_o.gif)](https://c2.staticflickr.com/2/1734/42744305102_f3e25b516f_o.gif)


## Points
#### Point concept
Point is a basement of 3 Dimensional data representation and its a core entity in Houdini. Understanding points allow understanding a huge part of SOP context (an area where you are creating models) in Houdini. To make things simple you can consider the single point as a complete geometry of any complexity.

**Point** in Houdini is a basic container in 3D space with a number of **attributes** associated with it.  
**Attributes** are just variables on points to store data. The minimal amount of point attributes is one: point position in the scene (`P`). Point position is a built-in [vector](Programming-basics#data-types) attribute, it holds 3 float number: point position in X, Y and Z in the global coordinate system (scene). 

The attributes could be **built-in** (standard, already existing in Houdini) and **custom** (created and defined by the user). You can examine point attributes and their values in **Geometry Spreadsheet** window.

Attributes syntax.  
You define an attribute in VEX with **data type**, `@` sign and **attribute name** :
- `v@myVectorAttribute;`
- `f@myFloatAttribute;`
- `i@myIntegerAttribute;`

You don't need to define the data type of built-in attributes:
- Point position: `@P;`
- Point number: `@ptnum;`
- Total amount of points: `@numpt;`

All modeling and bunch of other operations in Houdini are just around creating and managing points and their attributes.

#### Create a point
Create an [Attribute Wrangle](#create-a-wrangle-in-a-fresh-houdini-scene), set Run Over parameter to Detail.  
Now to create a point we need to know a [command](Programming-basics#commands) for that. Let`s use [Google Development Technique](https://github.com/kiryha/AnimationDNA/wiki/06-Tutorials#developing-with-google)!  

Search for "create point Houdini vex" and you will definitely find out that `addpoint()` will allow us to solve the task. Search for "addpoint vex" to read [Houdini documentation](http://www.sidefx.com/docs/houdini/vex/functions/addpoint.html) on `addpoint()` command.

Always **start from reading the documentation** for the command you wish to use, even if explanation there would not have any sense for you. This could happen at the beginning, but you will get more and more benefits from docs later, just do it to **build an experience**.

One of possible usage of this command is: `int  addpoint(int geohandle, vector pos)`  
What can we learn from this? 
- First, this command returns **point number** (integer data type). Its simple, if you have 10 points, each of them will have its own index starting from `0`, ending with `9`. You can get access to a particular point (and point attributes) using this index. In the case with one point, we will have one point number equal zero.
- addpoint command required 2 arguments (inputs): integer "geohandle" and vector "pos". We would not go deep into geohandle concept, imagine it as a port for plugging some data and use `0` for its value, it will suit for the majority of cases. Second argument "Pos" is a vector position in 3D space, X, Y and Z coordinates of the created point.

So with `addpoint()` we will create a point in a given position in space.  
Let's do it, write in your Attribute Wrangle node:

```C
// Create one point at the origin
addpoind(0,0);
```

Turn on **Display points** and **Display pint numbers**. If you did everything right you should get one point at the origin of Houdini scene:

[![](https://c1.staticflickr.com/1/882/26227913057_5d72523824_o.gif)](https://c1.staticflickr.com/1/882/26227913057_5d72523824_o.gif)

Check Geometry Spreadsheet to see point attributes: point number `0` has one vector attribute `P` which is "point position" and value of this attribute is `P.x = 0.0, P.y = 0.0, P.z = 0.0`

Curious minds might notice one issue: point position is a vector attribute, but we feed an integer as a second argument of addpoint() command. Despite our code works it is actually wrong, we never should mess up our data types! It's a good practice always to declare the data type explicitly. Let's also use a variable to organize our code better. Organizing one line of code may seem ridiculous but proper structuring and organization should be done to any amount of data we work with.

```C
// Create one point at the origin
vector position = {0,0,0};
addpoint(0, position);
```
Where `position = {0,0,0}` means X, Y, Z coordinates equal 0.

Read the next section "Manage attributes" if you keen to learn more about this topic or skip the additional theory and go to the next step and [create a line](#create-a-line)

##### Manage attributes
Ok, we create a point with a default position attribute.  
Let's learn how we can **create, set and get custom or built-in point attributes**. 

Create a new Attribute Wrangler, plug output of point creation wrangler to the first input, add a code: `@N;`. Check Geometry Spreadsheet to ensure that new built-in vector attribute `N` (normal) was added and has value `{0, 0, 0}`.

You can:
- initialize new attribute with value: `@N = {0, 1, 0};`
- set attribute value: `@N = set(0,0,0);`
- get attribute values: vector `v@getPosition = @P;` and float `f@getPosition_Y = @P.y;`  

You can use variables (`vector <variableName>`) instead of attributes (`@attrName`). What the difference between them? You can consider variables as a local data storage within current wrangle, variable you create in one wrangle would not be visible anywhere else. Attributes a stored globally and can be accessed by downstream nodes. 

If you don't need to access attribute outside the wrangle — use variables instead to keep the scene clean. 

#### Create a line
Learning such small and easy but fundamental thing as **point creation** we can do a lot of powerful things! Let's do the next step and **create 10 points along X-axis** to build a line! 

To create 10 points we need to repeat 10 times what we already learned — one point creation. Remember what programming concept we need to use in order to perform [repeating](Programming-basics#loops)? Our loop will run 10 times. Inside the loop, we will place a command to create a point:

```C
// Create 10 points
for (int n=0; n<10; n++){
    addpoint(0, set(0, 0, 0));   
    }
```

Where `int n=0; n<10; n++` is a description how many times we will run the code inside the loop: starting from 0, ending at 10, with a step of 1. Here `n` is a variable which define an [**iteration step**](Programming-basics#loop-iterations): 1,2,3,4 ... 10.

Check geometry spreadsheet, we get our 10 points. Now they all share the same position {0,0,0}.

[![](https://c1.staticflickr.com/1/809/40204608945_7ecb8fe916_o.gif)](https://c1.staticflickr.com/1/809/40204608945_7ecb8fe916_o.gif)

Next, we need to set a proper position for each point inside our loop to form a line. Our first point may stay at the origin `{0, 0, 0}` but each next point should have X coordinate increasing by the certain value — distance from the previous point: `{<distanse>, 0, 0}`. The most obvious and common way to define this increasing value is to use **iteration step** of the loop `n`:

```C
// Create 10 points with 1 unit distance between them along the X-axis
for (int n=0; n<10; n++){
    addpoint(0, set(n, 0, 0));   
    }
```
[![](https://c1.staticflickr.com/1/864/26227912787_cdf8426591_o.gif)](https://c1.staticflickr.com/1/864/26227912787_cdf8426591_o.gif)

We run through the loop 10 times and each next iteration gives us increasing by 1 point position at X coordinate (see P[x] in geometry spreadsheet). But what if we want to determine another then 1 incremental value for the X position? In other words, how we can modify the distance between points?

Since we are using **iteration step** value to determine coordinates we should modify this iteration step value to modify coordinates. So we will **multiply** the iteration step by **another value**, let`s call it **distance coefficient**. If distance coefficient would be less then 1 — the distance will decrease, and if it would be greater than 1 the distance will increase.

You should know that multiply a value by 0.5 it is the same as divide this value by 2, right? So we get increasing or decreasing of some value with the same multiplication operation. This is how we start using math to achieve our goals!

In this case, distance coefficient would be a constant (same value for each step of the loop iterations). And we can define it as a variable in our code:
```C
// Create 10 points with 0.5 unit distance between them along the X-axis
float distCoef = 0.5;

for (int n=0; n<10; n++){
    addpoint(0, set(n*distCoef, 0, 0));   
    }
```

Changing distance coefficient value in the Wrangle code is not very handy and interactive. So let`s create a UI element on the Wrangle node which will drive our distance:

```C
// Create 10 points along the X-axis
float distCoef = chf('Distance');

for (int n=0; n<10; n++){
    addpoint(0, set(n*distCoef, 0, 0));   
    }
```
Where `chf('Distance')` tells to take value from 'Distance' parameter on the Wrangle. To activate UI with this parameter you need to press a button with a slider icon right to the VEX Expression window. 

There are several options for creating UI in Wrangle node:
- Integer: `int integerValue = chi('Integer');`
- Float: `int floatValue = chf('Float');`
- Vector: `vector vectorValue = chv('Vector');`
- Modify parameter throug ramp: `float remapPosition_X = chramp('remap_P.x',@P.x);`

Ok, let`s define a number of points with UI also:
```C
// Create points along the X-axis
float distCoef = chf('Distance');
int numberOfPoints = chi('Number_Of_Points');

for (int n=0; n<numberOfPoints; n++){
    addpoint(0, set(n*distCoef, 0, 0));   
    }
```

To finalize line creation we need to connect our points with polygons to build an actual line. 

In Houdini, to create a polygon (primitive) you need to build a primitive and add vertexes to it. To build a primitive in VEX use `addprim()` command, to add a vertex to a primitive use `addvertex()` command. We need to create one primitive and add all our points to it.

Create another Wrangle (Run Over Points) and connect points creation wrangle as a first input. Enter the code:

```C
// Create LINE primitive
int primitive = addprim(0, 'polyline'); 
// Calculate total number of points
int numberOfPoints = @numpt;

// Create a vertex for each point in primitive
for (int n=0; n<numberOfPoints; n++){
    addvertex(0, primitive, n);  
    }
```

Here we create a primitive: `addprim()` and get a number of points we created in the first Wrangle node: `numberOfPoints = @numpt`, the `@numpt` is a built-in attribute which returns the total number of points from the first input. Then in the loop for each point we create vertex and add this ertex to a primitive: `addvertex()`.

# VEX first steps
#### Create a circle
Here we come to a more fancy stuff! At this point, we will start using trigonometry to draw a circle.

In the [line example](#create-a-line) we define the position of each point in 3D space using the [Cartesian coordinate system](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) by setting X, Y and Z values. This is the most intuitive and accustomed coordinate system, however not the only one. When it comes to a circle the [Polar coordinate system](https://en.wikipedia.org/wiki/Polar_coordinate_system) may work better.

See the corresponding chapter in [math basics](Math-basics) section for more details.

With the Polar coordinate system, you can define point position on the plane using **angle** and **distance** from the center. If the distant would be a constant value then we can set point position on the perfect **circle** by specifying only the **angle for each point**! 

However, Houdini uses a Cartesian coordinate system to determine objects transformations (position, rotation and scale), hence you need to feed `{X,Y,Z}` values to `addpoint()` function, it would not understand the angle. So after we define point positions of the circle in Polar coordinates (angle) we would need to convert them to Cartesian (X,Y,Z). And the relationship between Polar and the Cartesian coordinate systems can be expressed through sine and cosine functions: 

- position X = cosine(angle)
- position Y = sine(angle)

Very simple and elegant, right? The image from the Sine article on Wikipedia illustrates this dependency very well:

[![](https://c1.staticflickr.com/1/808/40204608805_2258ee27c4_o.gif)](https://c1.staticflickr.com/1/808/40204608805_2258ee27c4_o.gif)

In VEX code this part will look like:
```C
float angle = 0;
vector position = {0,0,0};
int numberOfPoints = 256;

for (int n=0; n<numberOfPoints; n++){
    position.x = cos(angle);
    position.z = sin(angle);
    addpoint(0, position);
```

It's time to speak about the **angle**. We can consider a **circle** as one **full turn** of a point around the origin. And we use to know that **angle of one turn** is equal to 360 degrees. Degrees (measurement of the angles) were designed for human usability and they are really good in this role, but in Math and computer graphics, you will have to deal with **Radians**.

Why Radians? Because radians is a mathematical angle definition based on circle attributes. One turn (360 degrees) is equal 2*[Pi](https://en.wikipedia.org/wiki/Pi) radians. Radians are all about the relation between circle radius and circumference:

[![](https://upload.wikimedia.org/wikipedia/commons/4/4e/Circle_radians.gif)](https://upload.wikimedia.org/wikipedia/commons/4/4e/Circle_radians.gif)

Let`s put theory into practice an build our magic circle finally! We will use the same concept as we did with the line: 

- define the **number of points** 
- define starting **angle** of rotation and position
- define a **segment rotation angle** for each point
- in the loop (for each of point number):
  * create a point 
  * set point **X and Y coordinates** through the **angle**
  * increment angle by segment rotation angle

The number of points could be either constant (12 points) or user-defined with UI variable:  
`int numberOfPoints = chi(number_of_points);`

A full turn of one point (circle) equal 2Pi radians. If we use more then one point, each point will need to rotate only by the fraction of 2Pi. In other words, the segment rotation angle for each point will be equal 2Pi divided by the number of points:  
`float segmentAngle = 2*3,14/numberOfPoints;`

The first point will have rotation value equal 0. For the second point rotation value, we will add segment rotation angle to a previous rotation value (0 + segment rotation angle). For the third point rotation value, we will add segment rotation angle to a previous rotation value (0 + segment rotation angle + segment rotation angle). And so on. In such way, we will increment angle by segment rotation angle:  
`angle = angle + segmentAngle;`

Lets put all together. Create attibute wrangle in **Detail mode** and enter code:
```C
// Initialize angle and position
float angle = 0;
vector position = {0,0,0};
// Get number of points from UI
int numberOfPoints = chi('number_of_points');
// Calculate angle for each point
float segmentAngle = 2*3.1415/numberOfPoints; 
   
// Build a circle
for (int n=0; n<numberOfPoints; n++){
    // Set position throuth angle
    position.x = cos(angle);
    position.z = sin(angle);
    // Create a point
    addpoint(0, position);
    // Increment rotation angle
    angle = angle + segmentAngle;
}
```
[![](https://c1.staticflickr.com/1/869/26230204217_e2312e5fb3_o.gif)](https://c1.staticflickr.com/1/869/26230204217_e2312e5fb3_o.gif)

#### Sine 
In the example above we used **sine** and **cosine** to build a circle. Let's take a closer look at a sine magic function. How can we imagine (visualize) this function in our scene?

First, we need to understand how math function works. Same as [functions](Programming-basics#functions) in programming math function produce some results (return values) based on input data (arguments). Check [wiki sine article](https://en.wikipedia.org/wiki/Sine):

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/Sine_one_period.svg/600px-Sine_one_period.svg.png)

Y = sin(X) means: for any input value (X) sine will calculate a particular output value Y. Here X is an argument of a sine function. Sine of 1/2 Pi will give you one.

Second, sine function returns a [float](Programming-basics#data-types) value. 

You can imagine a sine as a **global invisible power field** existing in your scene which can modify point positions by certain values according to its shape. What is the shape of this field? We see the sine graph, it's obviously a **wave**. Let's create a grid and apply the power of our field to visualize sine in the 3D scene!

Create a grid with a decent amount of rows and columns (say, 50) drop Attribute Wrangle node after grid and enter the code:

```c
@P.y = sin(PI*0.5);
```
This means: we are modifying Y position of all grid points by the value which will return a sine function of Pi divided by two. Since sine returns a float we need to use it with a float (Y) component of a vector (XYZ) position. 

What happened with the grid? Check Geometry Spreadsheet: Y coordinate of very point become equal one, in other words, we move grid one unit up. Everything is correct, according to a sine graph the argument of 1/2 Pi should return 1. But where is a wave?

Take a look at sine graph: we have X-axis and moving along this axis from 0 to 2Pi we get different values Y, returned by the sine function. This Y values based on constantly growing X values give us a wave shape. Currently, we feed a **constant** to our sine function as an argument (PI*0.5). Constant means a value which **does not change** under any **conditions**. So to get wave shape we have to feed to a sine some **varying** value — a value which will **change** over some **variation condition**. The most common example of such condition is **time**: time is a constantly growing value which you can feed as an argument to your function. VEX has built-in variable `@Time` which returns current time in seconds.

```c
@P.y = sin(@Time);
```

Press play: you grid is going up and down over the time. We visualize a sine function in our scene under the time condition. Works great but this is not we keen to get: currently, all grid points get the same value which changes over the time. To deform our grid (points) with the sine we have to find another condition which will take into account our points (and give an individual result for each point). 

Another widely used example of variation condition is a **point number** which you can get with a `@ptnum` built-in VEX variable. We can use point number as an argument to a sine function and the grid will be deformed, but the result would not be clean enough.

In our case, we need to find another variation condition related to points and it could be a point position. Since sine function required a float argument and point position is a vector we can use only one axis of position:

```c
@P.y = sin(@P.x);
```

And here we are! Our magic sine field visualized in the scene with a help of grid deformation. Curious minds may already have an idea how we can modify position and shape of this field, by modifying argument and sine output values:

```c
@P.y = sin(@P.x*chf('Period'))*chf('Amplitude');
```
[![](https://c2.staticflickr.com/2/1754/41855185615_93a6665981_o.gif)](https://c2.staticflickr.com/2/1754/41855185615_93a6665981_o.gif)

This sine investigation should clear for us how to work with math function: 
- Any function returns a value of certain data type (vector, float etc) 
- Some function may be required argument (input value) to produce a result
- You can use variable value to get a variable result
- Common variation conditions are: time `@Time` and point number `@ptnum`

#### Noise
Let`s examine one more interesting function without going into the math background: [noise](http://www.sidefx.com/docs/houdini/vex/functions/noise.html). What this function produces (returns) and what it requires as an argument to work? 

[![](https://c2.staticflickr.com/2/1826/42878139242_b03dccb888_o.gif)](https://c2.staticflickr.com/2/1826/42878139242_b03dccb888_o.gif)

According to a documentation, you can use point position as an argument and noise can return either float or vector data. There is no description of the output of the noise itself, but we can make an assumption based on the name: we will get some variations of output values depending on the position. In other words, depending on each point position noise function will return certain value for this point. Ok, but how this variation pattern looks like?

Instead of a thousand words, let's just take a look at the noise beast in our scene! Rather than deform geometry (modify @P attribute) we will literally paint points with noise values with a help of @Cd attribute.

`@Cd` is one of the built-in Houdini attributes and it`s responsible for the color values of points or primitives. You can see this values in the viewport on your geometry. This is an attribute of a vector data type: 

`@Cd = {<valueRed>, <valueGreen>, <valueBlue>}`

To **visualize the nose function with colors** create a grid with 200 rows and columns and drop Attribute Wrangle after the grid. Let's learn how we can use `@Cd` attribute, fill the grid with a black color:

```c
// Make geometry black
@Cd = {0, 0, 0};
```
Note, your grid become black in the viewport! You can try to make it red (or green, or blue) as a simple exercise or yellow if you gonna become a real developer. 

It is time to play with a noise function but what option should we choose from the variety of available in docs? Since color is a vector attribute, let's use a vector as a return data type, and vector position as an argument: `vector  noise(vector pos)` and link each point color with noise values:

```c
// Paint geometry with noise values
@Cd = noise(@P);
```
Nice! Here we visualize the noise function on our grid with a color. Curious minds may try to visualize nose with a geometry deformation (`@P = noise(@P);`) but the result would not be clear enough.

[![](https://c2.staticflickr.com/2/1785/42922412061_de666b1a24_o.gif)](https://c2.staticflickr.com/2/1785/42922412061_de666b1a24_o.gif)

Even now while you can see the noise pattern in the scene it is not obvious how it's designed and how we can use it. So we will simplify our color vector visualization and use black and white float data to discover the red channel of noise:

```c
// Paint geometry with noise values
@Cd.r = noise(@P);
```
Not quite what we can expect? Check our VEX developer best friend — Geometry Spreadsheet, two other components of our @Cd attribute (@Cd.g and @Cd.b) are equal 1, so we need to make them 0 before:

```c
// Make geometry black
@Cd = {0, 0, 0};
// Paint geometry with noise values
@Cd.r = noise(@P);
```
Better, but still... too blurry! We can learn from docs that noise function returns values in a range from 0 to 1. So each point gets value within this range according to the noise pattern. Let's make our visualization more contrast: if the value is lower than some value (and we can use the average value of our range here) it becomes black, otherwise, it becomes red. And average of our 0-1 range = (0 + 1)/2 = 0.5 

If... Otherwise... Sounds like we need to use a [condition](Programming-basics#conditions) programming concept! We will get a noise value and assign it to a variable named "noseValues". Float variable, because we will set one float channel `@Cd.r`. Then we will check if this value is greater then a threshold (average) then we will paint point to a red color. Otherwise, it will remain black and we don't need to set this up in our code. If you don't use `else` statement this means: otherwise — do nothing.

```c
// Make geometry black
@Cd = {0, 0, 0};
// Assign noise values to variable 
float noseValues = noise(@P);

// Paint geometry with noise values
if(noseValues > 0.5){
    @Cd.r = 1;
    }
```
Now we can see a clear noise pattern! Switch to Primitives and add UI elements to examine the noise function parameters interactively:

```c
// Make geometry black
@Cd = {0, 0, 0};
// Assign noise values to variable 
float noseValues = noise(@P*chf('Size') + chf('Offset'));

// Paint geometry with noise values
if(noseValues > chf('Threshold')){
    @Cd.r = 1;
    }
```

[![](https://c1.staticflickr.com/1/896/42204724394_e2b44bccd5_o.gif)](https://c1.staticflickr.com/1/896/42204724394_e2b44bccd5_o.gif)

How can we use noise function in production? For example, you can scatter points according to a noise pattern: add primitives to a [group](vex-snippets#create-groups) and use this group in scatter node. Or you can deform geometry with the noise pattern. Or anywhere you need controlled procedural variation!

So now we have **two methods to visualize VEX functions**: you can deform geometry as we did with a [sine](#sine) or paint geometry as in the current example.

# VEX basics
Check [VEX snippets](vex-snippets) for more VEX examples.