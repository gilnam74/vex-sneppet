# Introduction
If you want to learn VEX and Math beginning from the basics here is a good place to start. You need elementary knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops then you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

##### Create a wrangle in a fresh Houdini scene
- Press TAB in Node View, type `geometry`, hit ENTER
- Dive inside "Geometry" node, delete "File" node
- Press TAB, type `aw`, hit ENTER to create "Attribute Wrangle" node

If you need to **CREATE** data with Attribute Wrangle: switch **"Run Over"** parameter to **"Detail"**. If you will feed some data to Attribute Wrangle (connect any node to input) leave Run Over parameter at default Point state.

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

[![](https://lh3.googleusercontent.com/a5G6FyNkn9hBzSFp_PGe-2hU3ZUi41s2S9-Lce_MyS7K8krNhhdUBrqyaWHYwkjKF0SaI7X3kh_G1IYBM-T32mw4unS5uaZuyRoawaFf5vvCGvlx4JCiY1TuGsl1h-ggKb05vd5zgtRUXC30XSH8ZCM3g1lNHODhzpImw0M4HlXmWvORtHxXPEyyZx28ekDFw3ezPgkJJ8IZMllOLnmoaI9_z1v2HkTVK5ltOP_GcKwdPB-1T0Tf-0a0Gk_mg59-vLB4g7P9qbHww-hOh1Yv9Vs9OJSVGwhXCot015gkUfq6yK2wBrSJL3DnWotqTRF5rGSdB5VTeT6SO8Lm1MOSt0lfTQWtlAiWEy_gmxbX_28shg0dEpbMiNo6oP-st5ZiSz_TW1pIdjaIF7KEDKOAoK7wWwmfozDwrntJh-JiGy6388XXIxqmm4GV-zCncirQfpm2JqNiHrNR_bUKfZrfWqhRPH9nPoTdRJcbBNfX0rsLfWr450MkpNXWvx44fES-C7i71OThURtN_0vsEd1wJ7F-NK5e5JBDIIc1EHpBqhMvkjMexHQHEct4ZkuOuB9VY9swK5WkebWb_0_VYlmY0foUM_WvykdK4psoDAk=w1915-h700-no)](https://lh3.googleusercontent.com/a5G6FyNkn9hBzSFp_PGe-2hU3ZUi41s2S9-Lce_MyS7K8krNhhdUBrqyaWHYwkjKF0SaI7X3kh_G1IYBM-T32mw4unS5uaZuyRoawaFf5vvCGvlx4JCiY1TuGsl1h-ggKb05vd5zgtRUXC30XSH8ZCM3g1lNHODhzpImw0M4HlXmWvORtHxXPEyyZx28ekDFw3ezPgkJJ8IZMllOLnmoaI9_z1v2HkTVK5ltOP_GcKwdPB-1T0Tf-0a0Gk_mg59-vLB4g7P9qbHww-hOh1Yv9Vs9OJSVGwhXCot015gkUfq6yK2wBrSJL3DnWotqTRF5rGSdB5VTeT6SO8Lm1MOSt0lfTQWtlAiWEy_gmxbX_28shg0dEpbMiNo6oP-st5ZiSz_TW1pIdjaIF7KEDKOAoK7wWwmfozDwrntJh-JiGy6388XXIxqmm4GV-zCncirQfpm2JqNiHrNR_bUKfZrfWqhRPH9nPoTdRJcbBNfX0rsLfWr450MkpNXWvx44fES-C7i71OThURtN_0vsEd1wJ7F-NK5e5JBDIIc1EHpBqhMvkjMexHQHEct4ZkuOuB9VY9swK5WkebWb_0_VYlmY0foUM_WvykdK4psoDAk=w1915-h700-no)

Check Geometry Spreadsheet to see point attributes: point number `0` has one vector attribute `P` which is "point position" and value of this attribute is `P.x = 0.0, P.y = 0.0, P.z = 0.0`

Curious minds might notice one issue: point position is a vector attribute, but we feed an integer as a second argument of addpoint() command. Despite our code works it is actually wrong, we never should mess up our data types! It's a good practice always to declare the data type explicitly. Let's also use a variable to organize our code better. Organizing one line of code may seem ridiculous but proper structuring and organization should be done to any amount of data we work with.

```C
// Create one point at the origin
vector position = {0,0,0};
addpoint(0, position);
```
Read the next section "Manage attributes" if you keen to learn more about this topic or skip additional theory and go to the next step (create a line)[create-a-line]

##### Manage attributes
Ok, we create a point with a default position attribute.  
Let's learn how we can **create, set and get custom or built-in point attributes**. 

Create a new Attribute Wrangler, plug output of point creation wrangler to the first input, add a code: `@N;`. Check Geometry Spreadsheet to ensure that new built-in vector attribute `N` (normal) was added and has value `{0, 0, 0}`.

You can:
- initialize new attribute with value: `@N = {0, 1, 0};`
- set attribute value: `@N = set(0,0,0);`
- get attribute values: vector `v@getPosition = @P;` and float `f@getPosition_Y = @P.y;`  

And more:

```C
// Get attribute value:  

vector pointPos = v@opinput0_P;
// where <0> - first input, <P> - quarried attribute  

vector pointPos = point(0, "P", @ptnum);
// where <P> is quarried attr, <@ptnum> is a number of quarred point   

vector pointPos = point("op:../geometryName", "P", @ptnum); 
// get attribute value from non-connected nodes


// Create point attribute and set value:
setpointattrib(0, "<attributeName>", <pointNumber>, <valueToSet>, "set");
// Can be used to set point attributes in detail mode!

```

We used variables (`vector <variableName>`) instead of attributes (`@attrName`) in examples above. What the difference between them? You can consider variables as a local data storage within current wrangle, variable you create in one wrangle would not be visible anywhere else. Attributes a stored globally and can be accessed by downstream nodes. 

If you don't need to access attribute outside the wrangle — use variables instead to keep graph clean. 

#### Create a line
Learning such small and easy but fundamental thing as **point creation** we can do a lot of powerful things! Let's do the next step and **create several points** to build a line!

#### Create a circle
Here we come to a more fancy stuff, at this point, we will start using math to solve our tasks. 