# Quick Start Introduction
If you want to learn VEX and Math beginning from the basics here is a good place to start. You need basic knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

To create a wrangle in a fresh Houdini scene:

# VEX first steps
## Create a point

# VEX snippets
Here you will find isolated blocks of VEX code, each of them performs one certain small task.  

##### Create geometry from points array:
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