---
layout: default
title: Modifying a process
parent: REST Advanced
nav_order: 2
---

## Modifying an existing REST process
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

### Modifying the event data inside a process

### Creating a new process

