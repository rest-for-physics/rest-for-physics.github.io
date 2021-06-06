---
layout: default
title: Installing
nav_order: 2
---

## Installing REST

These instructions will get you a copy of the project up and running on your local machine in your home directory.

The recommended way to download a copy of REST will be to clone it using the corresponding git command.

```
cd
git clone git@lfna.unizar.es:rest-development/REST_v2.git
```

As soon as REST is under strong development phase the repository will be private, and access to the REST repository will be only granted on demand.
Before granting access, an account must be registered at the [Unizar Gitlab site](https://lfna.unizar.es). 
Then, you will need to contact the authors to request access to the code.

### Prerequisites

The only mandatory prerequisite of REST is ROOT6. Details on the installation of ROOT will be found at the [ROOT's official site](root.cern.ch). 
One may directly find binary distributions on its [download page](https://root.cern.ch/downloading-root). 
If not, try to compile and install it manually.

We provide a script inside `scripts/installation/installROOT.sh`, to automatize the process of downloading, compiling and installing a predefined version of ROOT in your local system.
If your system comes installed with all the [ROOT prerequisites](https://root.cern.ch/build-prerequisites) the installation using this script should be quite smooth.

Example of installing ROOT6 using REST installation script.

```
cd REST_v2/scripts/installation/  
./installROOT.sh  
```

The script will install a particular version of ROOT defined inside the script, and it will add to your bashrc a line to load ROOT each time you start a new terminal session.
ROOT will be installed at `$HOME/apps`. Feel free to modify the `installROOT.sh` script to choose a different installation directory or ROOT version.

Before starting the REST installation, make sure you are running the desired ROOT version and binary.

```
root-config --version
which root
```

### Installing

After ROOT6 has been installed in the system, the compilation of REST should be straight forward. 
Note that it is recommended to compile REST using the same version of g++ compiler used to compile ROOT.

Go to the root directory of your local REST repository, lets name it here `REST_SOURCE_PATH` and execute the following commands.

```
cd ~/REST_v2  
mkdir build
cd build
cmake .. -DINSTALL_PREFIX=../install/master/ 
make -j4 install
```

After all the compilation and installation process ends, you will end up with an installed REST version at `~/REST_v2/install/master/`.

Execute the following command to configure your `.bashrc` to load REST in your system environment each time you open a new shell terminal.

 ```
 echo "source ~/REST_v2/install/master/thisREST.sh" >> .bashrc
 ```


### Basic tests of the REST installation

After sourcing `thisREST.sh` you should see a message on screen similar to the following one.

```
  *****************************************************************************
  W E L C O M E   to  R E S T

  Commit  . d1a37c6c (2019-08-27 16.11.18 +0800)
  Branch/Version . v2.2.12_dev/v2.2.12
  Compilation date . 2019-08-27 18.53

  Installed at . /home/daq/REST_v2/install

  REST forum site  ezpc10.unizar.es (New!)
  *****************************************************************************
```

You can test now that REST libraries are loaded without errors inside the ROOT environment using the `restRoot` command.

```
restRoot

   ------------------------------------------------------------
  | Welcome to ROOT 6.14/06                http.//root.cern.ch |
  |                               (c) 1995-2018, The ROOT Team |
  | Built for macosx64                                         |
  | From tags/v6-14-06@v6-14-06, Dec 11 2018, 12.58.00         |
  | Try '.help', '.demo', '.license', '.credits', '.quit'/'.q' |
   ------------------------------------------------------------

Loading library . libRestFramework.dylib
```

### Compilation options

Different options can be passed to the `cmake` command to personalize the REST installation. The following options are available in REST.

* REST Options/features
    * **INSTALL_PREFIX** allows to define the destination of the final REST install directory. The default value is either "REST_v2/install/" (if you haven't installed REST) or the current REST path (if you already installed REST).
    * **REST_WELCOME** (Default. ON) : If dissabled no message will be displayed each time we call thisREST.sh.
    * **REST_GARFIELD** (Default. OFF) : Enables access to [Garfield++](https://garfieldpp.web.cern.ch/garfieldpp/) libraries in REST. Garfield code inside REST will be encapsulated inside `#if defined USE_Garfield` statements.
    * **SQL** (Default: OFF) : Enables the use of mysql libraries in REST. SQL code inside REST will be encapsulated inside `#if defined USE_SQL`.
    * **REST_EVE** (Default: ON) : Enables the use of libEve of ROOT which provides hardware accelerated 3D view of detector model and events within.

* REST Packages:
    * **REST_G4** (Default: OFF) : Adds executable `restG4` which carries out simulation with [Geant4++](http://geant4.web.cern.ch/) using REST style config file.
    * **REST_SQL** (Default: OFF) : Adds executable `restSQL` which allows to populate and update a SQL database extracting the metadata information from a REST file database.

* REST Libraries:
    * **RESTLIB_GEANT4** (Default: OFF) : Enables the use of Geant4 event type and analysis processes in REST. It does not require *Geant4*. But it allows to access previous `restG4` generated data.
    * **RESTLIB_RAW** (Default: ON) : Enables the use of Raw signal event type and Raw signal conditioning processes in REST.
    * **RESTLIB_DETECTOR** (Default: OFF) : Enables the use of Detector event type and event reconstruction processes in REST.
    * **RESTLIB_TRACK** (Default: ON) : Enables the use of Track event type and Track identification processes in REST.
    * **RESTLIB_AXION** (Default: OFF) : Enables the use of Axion event type and Axion signal calculation processes in REST.

To pass the options to cmake, one need to append "-DXXX=XXX" in the cmake command, for example: `cmake .. -DREST_WELCOME=OFF -DREST_G4=ON`. Once you explicitly set an option, your option choice will become the default choice for future `cmake` executions.

#### libtbb.so.2, needed by XXX/libImt.so, not found; undefined reference to `TParticle::Sizeof3D() const'

This happens when one wants to install REST with `sudo make install`. It will together report many 
similar messages. This is because the `LD_LIBRARY_PATH` is cleared when you temporary 
switched to administrator user. The solutions is: 

1. login as administrator to do all the operation. Remember to source the needed files before 
opeartion.

2. run `make` first. After compilation, run `sudo make install` to install.

#### undefined symbol XXX

If this happens during installation, this may be a bug of REST code. Contact the developers

If this happens when launching restManager, this may be a problem of ROOT libraries. 
We found that in some platform the ROOT binary are using unmatched version of interface of some system library. 
Try to complie ROOT manually, or change another ROOT binary distribution.

[**prev**](index.md)
[**next**](getting-started.md)
