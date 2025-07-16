---
layout: default
title: The restG4 package
nav_order: 80
has_children: true
permalink: /rest-g4
---
# The restG4 package
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
In this section, you will find advice for getting started with restG4, including how to run and access output files from simulations. Examples included in the base [restG4 repository](https://github.com/rest-for-physics/restG4/tree/master/examples) are also illustrated. 

The `restG4` package provides an executable named restG4 which refers to both REST and geant4 library. To install it, 
we must have Geant4 and REST mainbody installed. Then use commands `cmake` and `make`. restG4 is a single-executable program. By default it is installed in REST 
bin directory (${REST_PATH}/bin/). We can directly type `restG4` to start it.

### Getting started with the restG4 package

The package restG4 provides a Geant4 code that can be interfaced with REST using [TRestMetadata](https://sultan.unizar.es/rest/classTRestMetadata.html) specific structures and define the Geant4 simulation conditions through a RML file. We use the [TRestGeant4Metadata](https://sultan.unizar.es/rest/classTRestGeant4Metadata.html) class to collect all the initial simulation conditions, such as which and how many particules will be generated, from where they will be generated, what data will be stored in the output file, and few other options. Any additional details will be found at the [TRestGeant4Metadata](https://sultan.unizar.es/rest/classTRestGeant4Metadata.html) class description. The [TRestGeant4PhysicsList](https://sultan.unizar.es/rest/classTRestGeant4PhysicsLists.html) serves to define the physics that will be included in the Geant4 simulations, together with particle production energy cuts, and other physics settings. As any file being processed with REST we will need to define a [TRestRun](https://sultan.unizar.es/rest/classTRestRun.html) object with common run definitions, such as run tag or run type.

The required input files to use *restG4* as a simulation toolkit inside REST are

1. A RML file definning the run, simulation conditions, and physics lists to be used in Geant4,

2. and a GDML describing the geometry of the detector.


### Basic Usage
Typing `restG4 -h` will show all available options.

`restG4 simulation.rml` will launch a simulation using `simulation.rml` as the configuration file. The output file will
be saved to the specified path in the configuration.

### Overriding values from the RML

The CLI interface allows to set a few different parameters without having to modify the RML.

- `restG4 simulation.rml -o output.root` to specify the output file.
- `restG4 simulation.rml -n 1000` to set the number of processed events
  (`n` in Geant4's `/run/beamOn n` or `nEvents` in the rml configuration).
- `restG4 simulation.rml -g geometry.gdml` to specify the geometry file.

### The RML file definitions

In general terms, a RML file to be used with *restG4* must define the following sections, and structures.

```
<!-- We must, as usual, define the location where the REST output files will be stored -->
<globals>
   <parameter name="mainDataPath" value="${REST_DATAPATH}" />
</globals>

<!-- A TRestRun section to define few run parameters with a general run description. -->
<TRestRun>
    ...
</TRestRun>

<!-- A TRestGeant4Metadata section definning few parameters, generator, and storage. -->
<TRestGeant4Metadata>
    ...
</TRestGeant4Metadata>

<!-- A TRestPhysicsLists section definning the physics processes active. -->
<TRestGeant4PhysicsLists>
    ...
</TRestPhysicsLists>
```

Few basic working examples can be found at the [restG4 examples](https://github.com/rest-for-physics/restG4/tree/master/examples) directory. Those examples will be used to illustrate step by step the execution of *restG4* and the obtention of few parameters for analysis. The documentation found at the [TRestGeant4Metadata](https://sultan.unizar.es/rest/classTRestGeant4Metadata.html) class will help you construct a generic RML file to allow you define your particular simulation setup.

### The GDML detector geometry

The detector geometry is defined using GDML. Check the [GDML official website](http://gdml.web.cern.ch/GDML/) for further details on how to create or modify an existing GDML geometry. The GDML file defines the most revelevant parts of the detector, active region, vessel, shielding, etc.

**You may have a look at the [basic-geometries](https://github.com/rest-for-physics/basic-geometries) repository for a brief tutorial on geometry implementation with two basic geometries.**

On top of that, every [example](https://github.com/rest-for-physics/restG4/tree/master/examples) at the restG4 repository has implemented its own basic geometry that will allow you to start with a simple working geometry. Many times you may find the geometry divided into different files. In the case of the examples provided in REST we will find up to three files, a main file defining the geometry parameters, a second one implementing the different primitives, solids, volumes and physical volumes, in the geometry, and a third one to include different materials definitions. 

- **setup.gdml**: This is the main geometry file. It defines all the parameters you will use later to define the dimensions and/or material properties of the geometrical solids, volumes and physical volumes used by Geant4 (I.e. the size of the vessel, the gas volume, etc). This file includes the two other files that are used to define materials (`materials.xml`) and the definition of solids, volumes and physical volumes (`geometry.gdml`).

- **geometry.gdml**: This file defines the solids, volumes and physical volumes using the parameter values defined in `mySetupTemplate.gdml`, and materials defined at `materials.xml`.

- **materials.xml**: This file defines all the materials you need to define your geometry volumes (including gas mixtures). The materials.xml defined in the repository is just used as a basic example, if there are new materials you need for the definition of your own setup you will need to include them your personal geometry files. In any case, this file will be kept in the repository as a library of materials that are commonly used in simulations.

In principle, you can follow any other file scheme (i.e. using a unique file for all material and geometry definitions), as soon as the geometry is compatible with ROOT (i.e. it can be imported into TGeoManager ROOT class). 

For few geometry primitives, it might happen that are supported in Geant4 but not in ROOT, or viceversa. You will need to load the GDML geometry into ROOT TGeoManager class to verify the geometry will be properly imported into REST.

If the geometry is valid then we can load it into ROOT and visualize it using the following lines of code.

```
cd $REST_PATH/config/template/geometry/
~ root
[0] TGeoManager *geo = TGeoManager::Import("mySetupTemplate.gdml");
[1] geo->GetTopVolume()->Draw("ogl");
```

### Multithreading

`restG4` makes use of the multithreading capabilities of Geant4 and can be used in multithreading mode to significantly
increase simulation speed.

- `restG4 simulation.rml -t 8` will launch a simulation using 8 worker threads.

By default `restG4` is executed in serial mode (one single thread) and is equivalent to the `-t 0` option.

Simulation results are determined by the random seed and the number of threads, so using different number of threads
with the same seed will produce different results. Using the same seed and same number of threads produces the same
results.

The multithreading implementation of `restG4` is new and although it looks to be working properly it may have some bugs. If
you find one of such bugs, please post an issue [here](https://github.com/rest-for-physics/restG4/issues) with
the `multithreading` label.

We encourage our users to use the multithreading feature especially when performing exploratory simulations.

### Producing, visualizing and printing Geant4 REST generated data using the examples

Inside the file `REST_PATH/config/template/restG4.rml`, you will find three different examples that will allow you to generate and store event data. The file `restG4.rml` contains a unique TRestRun and TRestPhysicsLists sections, common to each simulation case, and different TRestGeant4Metadata sections each of them definning different simulation conditions. All these examples use the basic geometry described previously.

The following list describes briefly the examples available,

1. **NLDBD**: It produces neutrinoless double beta decay mode of 136Xe isotope. The events are launched using a volume generator in the gas.
2. **MuonShower**: It produces a cosmic muon shower using the measured energy and angular spectrum of muons on earth. The muons are randomly generated from a virtual wall placed on top of the detector.
3. **Cd109**: It simulates the radiactive decay of Cadmium 109 isotope at a fixed position inside the detector. Low energy gammas from the de-excitation of the daugther isotope interact inside the detector.

These working examples can be launched with *restG4* command. In these examples, the number of primaries to generate is only 100, but you can modify it to make a longer simulation by definning the environment variable *NEVENTS*. We must also assure that the *REST_DATAPATH* variable is defined and points to an existing directory with write access.

The restG4 command receives up to two arguments,

1. the RML file containning the TRestGeant4Metadata, TRestRun and TRestPhysicsLists sections,
2. and the name of the TRestGeant4Metadata section to be used.

The execution of *restG4* follows this scheme,

```
restG4 cfgFile.rml sectionName
```

being the first argument, `cfgFile.rml` mandatory, and the second argument, `sectionName`, optional. If the name of the section is omitted when launching *restG4*, the first TRestGeant4Metadata section found inside `cfgFile.rml` will be considered.


### Ending the simulation early

The simulation run will naturally end when the selected number of events have been processed. They can either be
specified in the RML file or via the `-n` flag on the CLI. However, the user can also specify additional conditions to
end the simulation early.

- `restG4 simulation.rml -e 1000` will signal the simulation to stop once 1000 events have been marked as valid (saved
  on the output file). The final simulation will probably have a slightly higher number of entries than the value
  specified in the case of multithreading as the worker threads may still be working on valid events, or they may not
  have been saved yet. The number of processed events (the equivalent of `nEvents`) will be adjusted to the real number
  of processed events, overriding the selected value of the user, so the resulting simulation would be equivalent to a
  plain simulation using this value as `nEvents`. This holds more accurately the large the `-N` parameter.
- `restG4 simulation.rml --time 1h20m30s` will signal the simulation to stop after 1 hour 20 minutes and 30 seconds have
  elapsed since the start. You can specify any value using this format such as `1h15m`, `30s`, etc.
- Sending the interrupt signal (typically via `CTRL+C`) will signal the simulation to stop, and it will attempt to save
  the results into disk before closing the process.

