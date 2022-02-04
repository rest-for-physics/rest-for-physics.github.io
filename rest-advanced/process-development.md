---
layout: default
title: REST processes development
parent: REST Advanced
nav_order: 2
---

## REST process development
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Sometimes a complex analysis will require to include new algorithms for event reconstruction and/or event data conditioning, and/or complex observable calculations that require of the event data at a particular stage of the data processing directly at the processing chain.

We encourage new contributions to be pushed to the official [REST repositories](https://github.com/rest-for-physics) after having read our [contribution guide](https://github.com/rest-for-physics/framework/blob/master/CONTRIBUTING.md). It is important to understand that REST provides a way to publish [versioned code](../rest-basics/rest-versioning.md), so that any REST modified code producing publishable results should be uploaded/pushed to the repository in order to warranty the proper integrity and traceability of the data generated with REST. From time to time, a new [official REST release]( http://doi.org/10.5281/zenodo.4922415) is tagged and users are encouraged to only use those version releases for public results.

### Adding new observable definitions inside a process

To customize our analysis, we start from modifing an object inheriting from a [TRestEventProcess](https://sultan.unizar.es/rest/classTRestEventProcess.html) class. We take an example of adding new analysis items in [TRestRawSignalAnalysisProcess](https://sultan.unizar.es/rest/classTRestRawSignalAnalysisProcess.html). Assume we are going to investigate signal peaks in an event. We would like to add a map of signal id to signal peaks. We also like to add an 
observable of mean peak value.

We add several lines in the cxx file:

```
//inside function ProcessEvent()(TRestRawSignalAnalysisProcess.cxx)
...
map<int,Double_t> signalpeakvalue;`  
int signalpeaksum = 0;`  
for (int s = 0; s < fSignalEvent->GetNumberOfSignals(); s++){
      TRestRawSignal *sgnl = fSignalEvent->GetSignal(s);
      signalpeaksum += sgnl->GetMaxPeakValue();
      signalpeakvalue[sgnl->GetID()] = sgnl->GetMaxPeakValue();
}

SetObservableValue("SignalPeakMean", signalpeaksum / fSignalEvent->GetNumberOfSignals());
SetObservableValue("SignalPeakValue", signalpeakvalue);
...  
```

After changing the source code, we need to switch to the build directory and type `make install`.
Then, TRestRawSignalAnalysisProcess will be working in a new way. We don't need to change the rml 
file or the header file. The class definition would remain unchanged. 

### Registering error or warning messages inside a process

Any metadata class is able to register one warning message and one error message. In order to register an error or warning inside the [TRestMetadata](https://sultan.unizar.es/rest/classTRestMetadata.html) class we need to call the `SetError` or `SetWarning` methods. Right now, we store only the latest message passed to those methods, but each call will increase a counter so that we are able to identify the number of times those methods were called.

A process itself is a metadata class, and therefore, inside a process we will be able to add those error, or warning, messages when some conditions are fulfilled. We will be able to set the error for the process itself (this) or any other metadata class available at [TRestRun](https://sultan.unizar.es/rest/classTRestRun.html). In a hypothetical case we could do the following inside our process:

```
if( nHits == 0 )
	this->SetError("The number of hits is zero!");

fReadout = <TRestDetectorReadout>GetMetadata();
if( fReadout->GetNumberOfChannels() != 512 )
	fReadout->SetWarning( "Process: DetectorSignalToHits. Number of channels is not 512!");
```

Then, each metadata class will get a collection of warnings an errors. Once a file was processed with REST we will be able to print out those collected errors through the [TRestRun](https://sultan.unizar.es/rest/classTRestRun.html) interface, using the `PrintErrors` and `PrintWarnings` methods.

```
restRoot myFile.root
[0] run0->PrintWarnings();
-- Warning : Found a total of 1 process warnings at thread 0

-- Warning : Class: TRestDetectorSignalToHitsProcess Name: signalToHits
-- Warning : Number of warnings: 96
-- Warning : Message: Last event id: 117619. Failed to find readout positions in channel to hit conversion.
```

In the previous example we see how the `SetWarning` method was called 96 times inside the [TRestDetectorSignalToHitsProcess](https://sultan.unizar.es/rest/classTRestDetectorSignalToHitsProcess.html). Telling us how many times the warning condition was fulfilled during the processing.

### Modifying the event data inside a process

### Creating a new process

