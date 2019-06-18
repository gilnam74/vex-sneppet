# Intro
This section contains pipeline related tutorials.

## Installation
Currently `Eve` designed to be run on Windows OS. Here is the list of things you need to have before pipeline will works.

Before running Houdini Pipeline Toolkit you need to install:
- Houdini 
- Python 2.7.5 x 64 (or later version) 
- pip  
- PySide
- GitHub Desktop

#### Install Houdini
Go to [SESI site](https://www.sidefx.com/products/compare/), choose, download and install your Houdini version. Eve assume Houdini would be located in `C:/Program Files/Side Effects Software/Houdini <buildNumber>/bin/houdinifx.exe`, otherwise you will need to tweak this path in `Eve/src/runHoudini.py` before creating any projects. 

#### Install Python
Get your [X64 Python 2.7.5](https://www.python.org/downloads/release/python-275/)

#### Install pip
To install PIP do the folowing:
* Download [get-pip.py](https://bootstrap.pypa.io/get-pip.py), place in Python folder, e.g `C:/Python27/` 
* Run cmd in `C:/Python27` folder and execute: `python get-pip.py`  
* Add pip path to a system environment variables: `PATH = c:/Python27/scripts`  

#### Install Pyside
Run cmd and exequte: `pip install PySide`

#### Install GitHub Desktop and clone pipeline
Install [Github Desktop](https://electronjs.org/apps/github-desktop) and clone this repo to the hard drive where you suppose to keep Eve pipeline, for example, `D:/Eve`. 

[![](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)

Alternatively, you can just download this repo as a zip file and extract to the same desired location. Hence you would not need GitHub Desktop but, in this case, it would be more tricky to get updates each time (download again, extract, overwrite existing files). 

 
## The first Eve project
Here we will step by step create an example project to understand the basics of Eve workflow. 

`Eve` presented as a folder with files on your hard drive and it does not include your project's data. For each of your project, we will need an additional folder usually located somewhere in "projects" directory, for example, `D:/projects/<projectName>`. Those project folders would be created automatically with Eve `Create Project` tool.

#### Create project
Go to the Eve root folder `P:/Eve` and run `createProjecrt.bat`:

[![](https://live.staticflickr.com/65535/48019770601_10f9642217_o.gif)](https://live.staticflickr.com/65535/48019770601_10f9642217_o.gif)

Select the project location (`P:/projects/`), enter the project name (`beauty`), Houdini build number you have installed and press the "Create Project" button. If everything went right, Windows Explorer will open a folder with a Houdini wrapper file (`runHoudini.bat`) located in `P:/projects/beauty/PREP/PIPELINE/`. Create a shortcut on Desktop to this file and launch Houdini by double-clicking it.