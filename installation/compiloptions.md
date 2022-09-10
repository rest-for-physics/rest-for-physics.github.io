---
layout: default
title: Compilation options
parent: Installation
nav_order: 25
---

### Compilation options

Different options can be passed to the `cmake` command to personalize the REST installation. The following options are available in REST.

* REST Options/features
    * **CMAKE_INSTALL_PREFIX** allows to define the destination of the final REST install directory. The default value is either "rest-framework/install/" (if you haven't installed REST) or the current REST path (if you already installed REST).
    * **REST_WELCOME** (Default. ON) : If dissabled no message will be displayed each time we call thisREST.sh.
    * **REST_GARFIELD** (Default. OFF) : Enables access to [Garfield++](https://garfieldpp.web.cern.ch/garfieldpp/) libraries in REST. Garfield code inside REST will be encapsulated inside `#if defined USE_Garfield` statements.
    * **SQL** (Default: OFF) : Enables the use of mysql libraries in REST. SQL code inside REST will be encapsulated inside `#if defined USE_SQL`.
    * **REST_EVE** (Default: ON) : Enables the use of libEve of ROOT which provides hardware accelerated 3D view of detector model and events within.

* REST Packages:
    * **REST_G4** (Default: OFF) : Adds executable `restG4` which carries out simulation with [Geant4++](http://geant4.web.cern.ch/) using REST style config file.
    * **REST_SQL** (Default: OFF) : Adds executable `restSQL` which allows to populate and update a SQL database extracting the metadata information from a REST file database.

* REST Libraries:
    * **RESTLIB_GEANT4** (Default: OFF) : Enables the use of Geant4 event type and analysis processes in REST. It does not require *Geant4*. But it allows to access previous `restG4` generated data.
    * **RESTLIB_RAW** (Default: OFF) : Enables the use of Raw signal event type and Raw signal conditioning processes in REST.
    * **RESTLIB_DETECTOR** (Default: OFF) : Enables the use of Detector event type and event reconstruction processes in REST.
    * **RESTLIB_TRACK** (Default: OFF) : Enables the use of Track event type and Track identification processes in REST.
    * **RESTLIB_AXION** (Default: OFF) : Enables the use of Axion event type and Axion signal calculation processes in REST.
    * **REST_ALL_LIBS** (Default: OFF) : It enables all REST libraries with a single flag. 

To pass the options to cmake, one need to append "-DXXX=XXX" in the cmake command, for example: `cmake .. -DREST_WELCOME=OFF -DREST_G4=ON`. Once you explicitly set an option, your option choice will become the default choice for future `cmake` executions.
