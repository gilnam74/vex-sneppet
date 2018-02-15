# Houdini  
[Project](#project-setup)  |  [16](#houdini-16)  | [ATTR](#attributes-and-parameters) |  [SOP](#sop)  | [Dynamic](#dynamic) | [Rendering](#rendering) | [Expressions](#expressions) |  [VEX](#vex) | [Glossary](#glossary)

## Project setup
### Houdini variables
Environment variables allow to setup variables and values in Houdini.  
List all available variables: `hconfig.exe -a`  
Check variable value in Houdini: in Texport window run `echo <$VAR_NAME>`  
Variables which works with paths divided into 2 types:  
- Location (HIP, JOB, etc)
- Path (HOUDINI_SCRIPT_PATH, etc)

`$JOB` - root for Houdini project.  
`$HIP` - path where Houdini scene is.  
`HOME` - Houdini settings for user

## Houdini 16
New features of H16

### Hotkeys: Scene view
- Reset rotate pivot to a viewport center: `Shift` + `Z`

### Hotkeys: Network view
- **Align** nodes: press `A`, press LMB, move left(or up)  
- **Move** node with **upstream** connections: `Shift` + `LMB` + move  
- **Move** node with **downstream** connections: `Ctrl` + `LMB` + move
- **Duplicate** node: `Alt` + `LMB` + move . 
- **Duplicate** node with **upstream** connections: `Alt` + `Shift` + `LMB` + move  
- **Duplicate** node with **downstream** connections: `Alt` + `Ctrl` + `LMB` + move  
- Set **display and render** flag: `R` + `LMB` click   
- Set **render** flag: `T` + `LMB` click   
- Set **bypass** flag: `Q` + `LMB` click  
- Set **template** flag: `W` + `LMB` click  
- **Reoder** inputs: `Shift` + `R`  
- Add **dot**: `Alt` + `LMB` click . 
- **Pin** the wire: `Alt` + `LMB` click  
- **Cut** the wire: `Y` + `LMB` drag  
- **Set** quickmark 1: `Ctr` + `1`  
- **Go to** quickmark 1: `1`  
- **Togle** quickmarks: tilda
### SOP changes
- `Point` SOP and `Vertex` SOP becomes an `Attribute Expression` 
- `Copy` SOP becomes a `Copy Stamp` 
- `Group` SOP replaced with new ones
- `Extrude` becomes `Extrude OLD`

## Attributes and parameters
[Attributes](http://www.sidefx.com/docs/houdini/model/attributes)  
[Global expression variables](http://www.sidefx.com/docs/houdini14.0/expressions/_globals)  
[Standard variables](http://www.sidefx.com/docs/houdini/nodes/sop/standardvariables)  
[Local SOP variables](http://www.sidefx.com/docs/houdini/nodes/sop/point#locals)


## SOP
Surface operation context
#### Scale PolyWire
Create `line`, add `UV Texture`, `Attribute Wrangle`, `Poly Wire`.  
Code in wrangle: `f@width = chf('Base_Width') * chramp('Width_Ramp', @uv.x);`  
Set Poly Wire width to `@width`

[![](https://lh3.googleusercontent.com/iFfN05awqdoim1Vj34c82rlPi--c71_jl5k4vNXqPbJ9ZFxAhvSynITrVHfoRWc1XRWztXnqH_eQqz-vHqdUZRCDzuES1ebX7zHz6pmdAzBFbIttzQz3F9T93yzraxm5jJERG6Y9ENnhNnpPJF5jJ_ALWv-BmjQGziv7b67qJlzViGswdFIiGuesJfHT6IwhOH-Miyvbmdn6WhhXFKstMyFtszztBOlivP0BLsn8Z5Tuyxut9IJBPiqSkkfhKoO47N6THESbEk5XIMu_CPt8wN5FV9252vD3Vt0lozLIi8hYxqorJrxaVylBLO2gLrdroMznmoh5aCLq3I7La4n1Rx3T780dCy8-HRWqtViDjWNeNpOfa9gAMMqL6ny1FjnjMGjBBGVlH9YI8m51brJ80SR80rZK59c9aVh7Vj5F6N7aIrEvJR6ewGnMBpeWF4tZCO0QxiDctOCZVMH7TVaoIcIyVkAj26Qy45mDsFTUXqFDTJP9XH28Vz_ELUc4X4XM9zpu0hXqUYkJEhMFWIsAX7kkZne5zPWTiiopx6TQ_JOg5semIn61W7ENon76jPKrFUtt_C_FA0Y=s2010-w2010-h1898-no)](https://lh3.googleusercontent.com/iFfN05awqdoim1Vj34c82rlPi--c71_jl5k4vNXqPbJ9ZFxAhvSynITrVHfoRWc1XRWztXnqH_eQqz-vHqdUZRCDzuES1ebX7zHz6pmdAzBFbIttzQz3F9T93yzraxm5jJERG6Y9ENnhNnpPJF5jJ_ALWv-BmjQGziv7b67qJlzViGswdFIiGuesJfHT6IwhOH-Miyvbmdn6WhhXFKstMyFtszztBOlivP0BLsn8Z5Tuyxut9IJBPiqSkkfhKoO47N6THESbEk5XIMu_CPt8wN5FV9252vD3Vt0lozLIi8hYxqorJrxaVylBLO2gLrdroMznmoh5aCLq3I7La4n1Rx3T780dCy8-HRWqtViDjWNeNpOfa9gAMMqL6ny1FjnjMGjBBGVlH9YI8m51brJ80SR80rZK59c9aVh7Vj5F6N7aIrEvJR6ewGnMBpeWF4tZCO0QxiDctOCZVMH7TVaoIcIyVkAj26Qy45mDsFTUXqFDTJP9XH28Vz_ELUc4X4XM9zpu0hXqUYkJEhMFWIsAX7kkZne5zPWTiiopx6TQ_JOg5semIn61W7ENon76jPKrFUtt_C_FA0Y=s2010-w2010-h1898-no)

#### Randomize copies with VEX
After `Copy to Points` node create: `Assemble` (Create packed geo = 1), `Point Wrangle`, `Unpack`. Code in wrangle:

```c
for(int i=0; i<npoints(0); i++){
    vector scale = {1,1,1} * fit01(rand(i+ch("seed")), ch('min'), ch('max'));
    
    matrix3 m = ident();
    scale(m,scale);
    setprimintrinsic(0,'transform',i,m);    
}
```

#### Randomize copies with VOP (orient)
After `Scatter` before `Copy to Points` nodes add `Attribute VOP`. In `Attribute VOP`:  
- **Scale**: geometryvopglobal1.ptnum > random > fit > bindExport (pscale)
- **Rotate**: geometryvopglobal1.ptnum >  random > fit > degtorad > rotate > matxtoquat > bindExport (4 floats): orient
  
[![](https://goo.gl/gfX8yz)](https://goo.gl/gfX8yz)[![](https://goo.gl/12qsfY)](https://goo.gl/12qsfY)

#### Randomize copies with VOP (rot)
Download [houdini file](https://drive.google.com/open?id=0B08-uC9HedKCZmttV1lnREo3dFE)

![](https://lh3.googleusercontent.com/PKrZU43aOLGyBWAV1RaiHLjQdlbqbqlk2Ken7LYR2MICGwoo0r2FxDMuRcqgB_SGBgn1EuIV405DoUoll2rnh0ww8MGXI5TaTQtR0DBMiYeHCkE_ClVRqO8JD5CItp0ywwA_YVZ82IWBRtd_qTLnylM_n3qYSmIVHGBLy8H4z6IZPPpnX8bdjhzxzykqXHkr_fKrC27qfkdfIKL0Hy6Eop8DpuJdHWLYmII9q9JH8Bgt54COo6TaPY_9feHml4c2cWQEC66L45UJNLRgT-EpbQCFlXn1I_Qs7UEOLfooBl634HxGcHy9a_KEOFj0dFLcd2ytGy13Lki1l9jPQvihbM1gSx8-MBvulQOCZdlU-ifS4y8TyoWzDURt79Us8j09WxajoqD3NJgfyh3t16UAdrtX4Y8dZWpczq4bph2Ms6IQdMw6rSZO9wUjcc_GQs05YHYmD8clEOFe796iOK1JZnG6Hav6mPseUyVk2qptKPE3aeInNcMRFvU28rxpAN6oR57xEKG8Sp_j2FkQJlxfW_GtjT5HhfEZYov9JamOOkaDhpjJBd-fc_bOpybUPZo4UgKMAYB4pWhphx3ernE-PTVKDEbZEheEXEyeCbGcjw=w1920-h820-no)


## Dynamic


## Rendering
### OUT context
In OUT Context create Mantra Node
In Mantra:
- Candidate objects: del `*` to render nothing
- Objects > Force Objects: set name of Node to render

## Expressions
Random scale: fit01(src value, result min, result max): `fit01(rand($PT), 1, 10)`

## VEX
##### Multiply distribution (make small smaller, big bigger):
```c
value = pow(value, 8.0);
```

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

##### VEX Rotate with @orient
```c
float rotate_Y = chf('rotate_Y'); 

v@N; 
v@up = {0,1,0}; 
p@orient = set(0,rotate_Y,0,1);
```
Download [houdini file](https://drive.google.com/open?id=1noAA4z1-tBeOfty7rbkoic1cR5xDO6wT)

##### VEX circle
```c
int segments = chi('number_of_points');
float radius = chf('radius');
float step = 2*3.1415/segments; // 2Pi devided by number of points. Angle of one segments in radians
float angle = 0;
vector pos = {0,0,0};
    
for (int i=0; i<segments; i++){

    //polar to cartesian
    pos.x = cos(angle)*radius;
    pos.z = sin(angle)*radius;
    
    addpoint(geoself(), pos);
    angle += step;
}
```
Download [houdini file](https://drive.google.com/open?id=1c0ZNDunZ6XQF-k1uTYNqnC34SQFCYbgG) with phyllotaxis, spirals and circle.

## Glossary
Advection — movement of fields (e.g density or temperature) along velocity vectors