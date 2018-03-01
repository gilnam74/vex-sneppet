# Quick Start Introduction
If you want to learn VEX and Math beginning from the basics here is a good place to start. You need basic knowledge of Houdini UI and understanding of core programming concepts. If you can render metal sphere in Houdini and know what is syntax, data types, variables and loops you are fine to go (check [Programming Basics tutorial](Programming-basics) if not).

To create a wrangle in a fresh Houdini scene:
- Press TAB in Node View, type'geometry', hit ENTER
- Dive inside "Geometry" node, delete "File" node
- Press TAB, type "aw", hit ENTER to create "Attribute Wrangle" node

If you need to **CREATE** data with Attribute Wrangle: switch **"Run Over"** parameter to **"Detail"**. If you will feed some data to Attribute Wrangle (connect any node to input) leave Run Over parameter at default Point state.

# VEX first steps
## Create a point
