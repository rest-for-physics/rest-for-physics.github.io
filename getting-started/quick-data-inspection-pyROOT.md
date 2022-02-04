---
layout: default
title: Using python
parent: Quick REST file inspection
grand_parent: Getting started
nav_order: 20
---

## Using a python script with pyROOT to access REST files
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

### Required python headers

When we write our python script we need to import the ROOT libraries, and any REST library we used to generate or process our files. Some examples of REST available libraries are: `libRestRaw.so`, `libRestDetector.so`, `libRestGeant4`, `libRestTrack`, and/or `libRestAxion`. On top of that we need always to include the main or core library, `libRestFramework.so` that loads the common REST classes.

The following header should be always included in your script adding a line for each library you will need.

```python
#!/usr/bin/python3

import ROOT

# We load the REST libraries
ROOT.gSystem.Load("libRestFramework.so")

# We are reading simulation data we will need at least the Geant4 library
ROOT.gSystem.Load("libRestGeant4.so")
```

### Opening a ROOT file generated with REST

Once the required libraries have been loaded, the first main class we need to know in REST is [TRestRun](https://sultan.unizar.es/rest/classTRestRun.html). TRestRun defines few helper methods that centralize the access to the event data, the analysis tree and event data. It can be used to open a ROOT file generated with REST, and access the data in a coherent way.

The following example shows how to create a `TRestRun` instance , named *rn* in this script, print the run metadata information, and get the number of event entries stored inside the file.

```python
# We open a ROOT file generated by REST
rn = ROOT.TRestRun("Run_g4Analysis_5MeV_1um.root" )

# We print the information of the run metadata object
rn.PrintMetadata()

# We get the number of entries
nEntries =  rn.GetEntries()
print ( "The number of entries is : " + str( nEntries ) )
```

### Retrieving the file metadata objects using TRestRun

The following example uses the already existing *rn* instance to retrieve a list of metadata objects found inside the file, prints the list with the metadata name together with its specific metadata class name, and calls the `PrintMetadata` method (present at any metadata class) to print information regarding one of the metadata objects in the list.

We use here the analysis file generated with restG4 [08.Alphas example](https://github.com/rest-for-physics/restG4/tree/master/examples/08.Alphas).

```python
# We retrieve the metadata object names registered inside the file
mdNames = rn.GetMetadataStructureNames()

# We iterate over the list and print the name together with the object class name
print("\n")
print ("The following metadata objects are found inside the file")
print ("--------------------------------------------------------")
for md in mdNames:
    print ( md + " is: " + rn.GetMetadata( md ).ClassName() )
print("\n") 

# We print the metadata information from the second element in the list
print ("We print the metadata information of the second element in the list:" )
print ("--------------------------------------------------------------------")
rn.GetMetadata( mdNames[1] ).PrintMetadata() 
```

### Accessing the data stored at a specific event entry

From the *rn* instance we may also get access to the event data and the analysis tree data. We just get the pointers to those objects using the `TRestRun` methods. Then, we may get any entry number found inside the file. Note that the entry number is just the position of the entry within the file, but it does not serve to fully identify the event. The event ID, which might take any integer value, is unique, and it can be used to identify an event between different files.

When we request an entry or event id to the `TRestRun` instance, the event pointer, named here *g4Ev*, and the analysis tree pointer, named here *aT*, will change their memory address to point to the location of the event entry we specified using the `TRestRun` methods: `GetEntry(N)`, `GetNextEntry()` or `GetEventWithID(id)`.

In this example we print the event data and analysis tree observables from 3 different event entries.
```python
# We retrieve the event pointer
g4Ev = rn.GetInputEvent()

# We retrieve the analysisTree pointer
aT = rn.GetAnalysisTree()

# There are in total 9421 entries. We get an arbitrary entry in between
rn.GetEntry(1213)
g4Ev.PrintEvent()
aT.PrintObservables()

# We get the next entry. It should be entry 1214.
rn.GetNextEntry()
g4Ev.PrintEvent()
aT.PrintObservables()

# We get the event with id 1858. Probably does not correspond with entry 1858
rn.GetEventWithID(1858)
g4Ev.PrintEvent()
aT.PrintObservables()
```

Note that in order to register the event data inside our analysis file it is necesary to enable the parameter `outputEventStorage`. If this parameter is not specified, its default will be normally `on`.

Obviously, now we could iterate over all the events to get specific information and perform a dedicated analysis using the event or analysis tree methods.

```python
for n in range(nEntries):
	rn.GetEntry(n)
	##
	## We do whatever we need with g4Ev and aT for each event entry
	##
```