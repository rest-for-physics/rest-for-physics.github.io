---
layout: default
title: Installation
nav_order: 20
has_children: true
permalink: /installation
---


The instructions in this section will get you a copy of the project up and running on your local machine in your home directory.

### Prerequisites for building REST
{: .no_toc }

The only mandatory prerequisite of REST is ROOT6. Details on the installation of ROOT will be found at the [ROOT's official site](https://root.cern.ch). 
One may directly find binary distributions on its [download page](https://root.cern.ch/downloading-root). 
If not, try to compile and install it manually.

{: .warning} The best choice is to compile ROOT6 from source. If ROOT6 compiles without problems, REST-for-Physics will do it.


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

### Main framework compilation and installation
{: .no_toc }

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

After all the compilation and installation process ends, you will end up with an installed REST version at `~/rest-framework/install/master/`.

Execute the following command to configure your `.bashrc` to load REST in your system environment each time you open a new shell terminal.

 ```
 echo "source ~/rest-framework/install/master/thisREST.sh" >> .bashrc
 ```

### Adding libraries to the REST compilation
{: .no_toc }

The REST framework provides only the structure and support to create and use REST libraries. Few official REST libraries are maintained by the REST community at the [REST-for-Physics](https://github.com/rest-for-physics) namespace. Please, refer to the respective repositories and README.md documentation to get more insights about the features and functionalities of each library.

By listing the contents of the *library* directory inside `rest-framework` (once you executed `pull-submodules.py`) you will quickly identify the available libraries. In order to enable a particular library, just get the library directory name, and use it to define a compilation flag as `-DRESTLIB_NAME`.

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

**Remark:** Notice that once we pass an option to cmake, that option will be cached inside the cmake system. I.e. we do not need to provide the installation path we provided the first time, and any future calls to `cmake` will assume `detector` and `raw` libraries are enabled.

1. TOC
{:toc}
