---
layout: default
title: Installation
nav_order: 20
has_children: true
permalink: /installation
---


The instructions in this section will get you a copy of the project up and running on your local machine in your home directory.

## Prerequisites for building REST
{: .no_toc }

The only mandatory prerequisite of REST is ROOT6. Details on the installation of ROOT will be found at the [ROOT's official site](root.cern.ch). 
One may directly find binary distributions on its [download page](https://root.cern.ch/downloading-root). 
If not, try to compile and install it manually.

We provide a script inside `scripts/installation/installROOT.sh`, to automatize the process of downloading, compiling and installing a predefined version of ROOT in your local system. If your system comes installed with all the [ROOT prerequisites](https://root.cern.ch/build-prerequisites) the installation using this script should be smooth.

Example of installing ROOT6 using REST installation script.

```
cd rest-framework/scripts/installation/  
./installROOT.sh  
```

The script will install a particular version of ROOT defined inside the script, and it will add to your bashrc a line to load ROOT each time you start a new terminal session.
ROOT will be installed at `$HOME/apps`. Feel free to modify the `installROOT.sh` script to choose a different installation directory or ROOT version.

Before starting the REST installation, make sure you are running the desired ROOT version and binary.

```
root-config --version
which root
```


## Adding libraries (TODO needs revision - These ideas should be moved to REST-advanced)

The concrete analysis tasks and experiment setups of REST are kept in individual libraries or packages. 
The main framework of REST only keeps general event types and analysis algorithms. We need to install
concrete library(package) to enable the workload, after installing REST mainbody.

Both `Library` and `Package` are kind of c++ projects that based on REST. The concept is to make 
that part of code envolve independently with REST framework, reducing the changing of REST framework.
They are usually kept in different git repositories. `Library` provides a library with new event types and 
analysis algorithms. `Package` provides not only libraries but also executables. They are installed to
REST installation path.

Some of the libraries/packages are provided as git submodule of REST, and can be installed together 
with REST framework, just by adding compilation options to cmake.

The following is a list of REST libraries/packages:

Name         | Type       |  cmake flag (=default)  | repository
-------------|------------|-------------------------|------------
restMuonLib  |   library  |                         | https://gitlab.pandax.sjtu.edu.cn/pandax-iii/restmuonlib
restDecay0   |   library  |   -DREST_DECAY0=OFF     | 
RestAxionLib |   library  |   -DREST_AXION_LIB=OFF  | https://lfna.unizar.es/iaxo/RestAxionLib
RestGeant4Lib|   library  |   -DREST_GEANT4_LIB=ON  | https://lfna.unizar.es/rest-development/RestGeant4Lib
restG4       |   package  |   -DREST_G4=OFF         | 
restP3DB     |   package  |                         | https://gitlab.pandax.sjtu.edu.cn/pandax-iii/restp3db
restSQL      |   package  |   -DREST_SQL=ON         | https://lfna.unizar.es/rest-development/restsql
restWeb      |   package  |                         | https://lfna.unizar.es/rest-development/restWeb

Note that if you changed the code, the **file modification** and **file deletion** will be reverted, but the **file creation** will be kept. 

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
