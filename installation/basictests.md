
---
layout: default
title: Testing the installation
parent: Installation
nav_order: 23
---


## Basic tests of the REST installation
{: .no_toc }

After sourcing `thisREST.sh` you should see a message on screen similar to the following one.

```
  *****************************************************************************
  W E L C O M E   to  R E S T

  Commit  . d1a37c6c (2019-08-27 16.11.18 +0800)
  Branch/Version . v2.2.12_dev/v2.2.12
  Compilation date . 2019-08-27 18.53

  Installed at . /home/daq/rest-framework/install

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
