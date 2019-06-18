# Intro
This section contains pipeline related tutorials.

## Installation
Currently, Houdini Pipeline Toolkit designed to be run on Windows OS. Here is the list of things you need to have before pipeline will works.

Before running Houdini Pipeline Toolkit you need to install:
- Houdini  
- Python 2.7.5 x 64 (or later version) 
- pip  
- PySide
- GitHub Desktop

#### Install Houdini
Go to [SESI site](https://www.sidefx.com/products/compare/), choose, download and install your Houdini version.

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
Install [Github Desktop](https://electronjs.org/apps/github-desktop) and clone this repo to the local drive, 
where you supposing to keep Eve pipeline. 

[![](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)](https://live.staticflickr.com/65535/48019681856_fd0a55facb_o.gif)

Alternatively, you can just download this repo as a zip file and extract to the desired location. Hence you would not need GitHub Desktop but, in this case, it would be more tricky to get updates. 

 
## Create the first project
* Download [HPT repository](https://github.com/kiryha/Houdini) as a ZIP file, extract to any temporary location.
* Modify `houdini` variable in runHoudini.py to match your Houdini install dir
* Run createProjecrt.but and follow instructions