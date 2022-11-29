---
layout: default
title: Installation
nav_order: 20
has_children: true
permalink: /installation
---


The instructions in this section will get you a copy of the project up and running on your local machine in your home directory.

{: .note } 
> The instructions found on this page should be common to any system based on UNIX. Check [Windows specific installation instructions here](https://rest-for-physics.github.io/installation/windows.html).

### Prerequisites for building REST

The only mandatory prerequisite of REST is ROOT6. Details on the installation of ROOT will be found at the [ROOT's official site](https://root.cern.ch). 
One may directly find binary distributions on its [download page](https://root.cern.ch/downloading-root), although **the best choice is to compile ROOT6 from source**. If ROOT6 compiles without problems, REST-for-Physics will usually compile without major issues.

We provide a script `installROOT.sh` inside the directory `scripts/installation/` to automatize the process of downloading, compiling and installing a predefined version of ROOT in your local system. If your system comes installed with all the [ROOT prerequisites](https://root.cern/install/dependencies/) the installation using this script should be smooth. Problems during the ROOT compilation are usually solved by indicating the appropriate paths for python, or using the appropriate compilation flags. It is recommended to have a look to the `installROOT.sh` script to identify the REST community recommended ROOT version, and the compilation flags that we tipically use.

Thus, downloading, compiling and installing ROOT6 using the REST installation script is as simple as execute the following and cross your finger.

```
cd rest-framework/scripts/installation/  
./installROOT.sh  
```

This script may fail, if it is the case, do not run the script again, just go to the generated build directory, usually at `$HOME/apps/root-xxx/build`, and try to compile again using `cmake ..` and `make -j install`. Copy/paste the errors on google to identify the cause of the error, or direclty post your issue at the [ROOT Forum](https://root-forum.cern.ch) they will reply quickly,

If the script is sucessful it will install a particular version of ROOT defined inside the script, and it will add to your `.bashrc` a line to load ROOT each time you start a new terminal session.

ROOT will be installed at `$HOME/apps`. Feel free to modify the `installROOT.sh` script to choose a different installation directory or ROOT version.

Before starting the REST installation, make sure you are running the desired ROOT version and binary.

```
root-config --version
which root
```

### Main framework compilation and installation

After ROOT6 has been installed in the system, the compilation of REST should be straight forward. 
Note that it is recommended to compile REST using the same version of g++ compiler used to compile ROOT.

Go to the root directory of your local REST repository, lets name it here `REST_SOURCE_PATH` and execute the following commands.

```
cd ~/rest-framework
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=../install/master/ 
make -j4 install
```

{: .note }
Note that once we have passed an option by argument to **cmake**, that option will be cached inside the cmake system. I.e. we do not need to provide the installation path in any future calls, **cmake** will just remember the last choice as soon as the **build** directory is not erased. Sometimes, when running into problems with the **cmake** configuration it is an option to completely remove the **build** directory to avoid problems with non-appropriate cached variables.

After all the compilation and installation process ends, you will end up with an installed REST version at `~/rest-framework/install/master/`.

Execute the following command to configure your `.bashrc` to load REST in your system environment each time you open a new shell terminal.

 ```
 echo "source ~/rest-framework/install/master/thisREST.sh" >> .bashrc
 ```

### Adding libraries to the REST compilation

The REST framework provides only the structure and support to create and use REST libraries. Few official REST libraries are maintained by the REST community at the [REST-for-Physics](https://github.com/rest-for-physics) GitHub organization. Please, refer to the respective repositories and README.md documentation to get more insights about the features and functionalities of each library.

Listing the contents of the *source/libraries* directory inside `rest-framework` (once you executed `pull-submodules.py` following instructions at the [downloading section](https://rest-for-physics.github.io/downloading.html)) you will quickly identify the available libraries. In order to enable a particular library, just get the library directory name, and use it to define a compilation flag as `-DRESTLIB_NAME`.

For example, in order to compile REST including the `detector` and `raw` libraries, you should update the compilation system set-up by moving again to the build directory and executing:

```
cd build
cmake .. -DRESTLIB_DETECTOR=ON -DRESTLIB_RAW=ON
make -j4 install
```

If you wish to compile REST-for-Physics with all the public available libraries you may use the `REST_ALL_LIBS` compilation flag.

```
cd build
cmake .. -DREST_ALL_LIBS=ON
make -j4 install
```

### Loading and testing the installation

Once the REST installation succeeds we may load the REST libraries by invoking the `thisREST.sh` bash shell script.

```
source /path/to/installation/thisREST.sh
```

If the welcome message was not disabled using the corresponding cmake option, you should now see the following output on screen

```
  *****************************************************************************
  W E L C O M E   to  R E S T  
  
  Commit  : 5ca24fea (2022-11-28 14:00:19 +0100)  
  Branch/Version : master/v2.3.13  
  Compilation date : 2022-11-29 15:30  
  Official release : No 
  Clean state : Yes 
```

This output can be generated again using `rest-config --welcome`.

{: .note }
The `rest-config` command will provide information on the specific installation options of your REST installation.

Now you may start `restRoot` to check that libraries are properly loading inside a ROOT interface.

```

~$ restRoot
= Loading libraries ...
 - /home/jgalan/rest-framework/install/lib/libRestFramework.so
 - /home/jgalan/rest-framework/install/lib/libRestAxion.so
 
root [0] 
```

{: .important}
The script `thisREST.sh` will load into your system **ROOT**, **Geant4**, **Garfield** and any other optional libraries required during the compilation. It is **important** to understand that this script loads exactly the same version of the ROOT libraries used for compilation, and that a REST compilation will only work properly when loading exactly the same version of ROOT used to compile REST.

{: .hint }
If you are working in a system with an official pre-installed release of REST, the most convenient during the compilation of your own REST build is that, before running cmake, you load the `thisREST.sh` from the latest official release, so that you load in your environment the required **Geant4**, **ROOT** and **Garfield** installations that are known to work properly.


1. TOC
{:toc}
