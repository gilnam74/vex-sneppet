# Quick Start Introduction
If you want to learn VEX and Math beginning from the basics here is a good place to start. You need elementary knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops then you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

##### Create a wrangle in a fresh Houdini scene
- Press TAB in Node View, type `geometry`, hit ENTER
- Dive inside "Geometry" node, delete "File" node
- Press TAB, type `aw`, hit ENTER to create "Attribute Wrangle" node

If you need to **CREATE** data with Attribute Wrangle: switch **"Run Over"** parameter to **"Detail"**. If you will feed some data to Attribute Wrangle (connect any node to input) leave Run Over parameter at default Point state.

# VEX first steps
The goal here is to start with the very simple and basic tasks keeping the amount of code minimal and gradually, step by step increase complexity of exercises. If you will really understand the basics it would be easy to develop extra functionality for the staring code.

## Create a point
Point is a basement of 3 Dimensional data representation and its a core entity in Houdini. Understanding points allow understanding SOP context in Houdini almost entirely. To make things simple for understanding you can consider the single point as a complete geometry of any complexity.

**Point** in Houdini is a basic container in 3D space with a number of attributes associated with it. The minimal amount of attributes is one: point position in the scene (`P`). Point position is a [vector](Programming-basics#data-types) attribute, it holds 3 float number: point position in X, Y and Z in the global coordinate system (scene). You can create another standard (Normal, Color, Velocity) or custom attributes of different data types for each point. All modeling and bunch of other operations in Houdini are just around creating and managing points and their attributes.