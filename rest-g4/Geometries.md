---
layout: default
title: Geometries example
parent: The restG4 package
nav_order: 62
---

## Geometries Example
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

### Running the Simulation

Run this example with:
```
restG4 run_file.rml
```
The `.rml` file is set to generate 100 events, to override this, simply include `-n 1000` in the terminal line when you run the simulation to run 1000 events instead. 

Once successfully run (this may take a few mintues), the output `root` file will be saved in the local directory under something like `Run00001_Name_Test.root`.


### Visualizing the Geometry

The geometry can be accessed by entering:
```
restRoot -m 1 output_file.root
```
to open a root session. Then, using `REST_Geant4_ViewEvent("fileName.root")` allows for the visualization of the geometry and particle tracks.


### Accessing the Output

The output file can be accessed by opening a root session with `restRoot -m 1 output_file.root` and opening a `new TBrowser`.


