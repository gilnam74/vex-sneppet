# Introduction
If you want to learn VEX and Math beginning from the basics here is a good place to start. You need elementary knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops then you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

To understand **applied Math** in 3D using VEX in Houdini our mission here.

##### Create a wrangle in a fresh Houdini scene
- Press TAB in Node View, type `geometry`, hit ENTER
- Dive inside "Geometry" node, delete "File" node
- Press TAB, type `aw`, hit ENTER to create "Attribute Wrangle" node

If you need to **CREATE** data with Attribute Wrangle: switch **"Run Over"** parameter to **"Detail"**. If you will feed some data to Attribute Wrangle (connect any node to input) leave Run Over parameter at default Point state.

All exercises from this chapter you can find in [VEX snippets hip file](https://drive.google.com/open?id=1qgdJa9TuYlrWgDfXWkMcpQcZu_DjC6kQ).

# VEX first steps
The goal here is to start with the very simple and basic tasks keeping the amount of code minimal and gradually, step by step increase complexity of exercises. If you will really understand the basics it would be easy to develop extra functionality for the staring code.

## Points
#### Point concept
Point is a basement of 3 Dimensional data representation and its a core entity in Houdini. Understanding points allow understanding a huge part of SOP context in Houdini. To make things simple you can consider the single point as a complete geometry of any complexity.

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

Where `int n=0; n<10; n++` is a description how many times we will run the code inside the loop: starting from 0, ending at 10, with a step of 1. Here `n` is a variable which define a **iteration step**: 1,2,3,4 ... 10.

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

Since we are using **iteration step** value to determine coordinates we should modify this iteration step value to modify coordinates. So we will **multiply** the iteration step (which is currently equal our distance) by **another value**, let`s call it **distance coefficient**. If distance coefficient would be less then 1 — the distance will decrease, and if it would be greater than 1 the distance will increase.

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
- Float: `int floatValue = chi('Float');`
- Vector: `vector vectorValue = chv('Vector');`
- Modify parameter throug ramp: `float remapPosition_X = chramp('remap_P.x',@P.x);`

Ok, let`s define a point number with UI also:
```C
// Create points along the X-axis
float distCoef = chf('Distance');
int pointNumber = chi('Number_Of_Points');
for (int n=0; n<pointNumber; n++){
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

It's time to speak about the **angle**. We can consider a **circle** as one **full turn** of a point around the origin. And we use to know that **angle of one turn** is equal to 360 degrees. Degrees (measurement of the angles) were designed for human usability and they are really good in this role, but in Math and computer graphics, you will have to deal with **Radians**.

Why Radians? Because radians is a mathematical angle definition based on circle attributes. One turn (360 degrees) is equal 2*[PI](https://en.wikipedia.org/wiki/Pi) radians. Radians are all about the relation between circle radius and circumference:

[![](https://upload.wikimedia.org/wikipedia/commons/4/4e/Circle_radians.gif)](https://upload.wikimedia.org/wikipedia/commons/4/4e/Circle_radians.gif)

Let`s put theory into practice an build our magic circle finally! We will use the same concept as we did with the line: 

- define the number of points 
- in the loop (for each of point number):
  * create a point 
  * set point X and Y coordinates through the angle keeping the same distance from the origin

The only thing we are missing now in our [pseudocode](Programming-basics#pseudocode) is an increment (iteration step) definition. With the line example, we use iteration step number of the loop directly: in each iteration, set X coordinate of the newly created point equal to iteration step number.





#### Sine 
