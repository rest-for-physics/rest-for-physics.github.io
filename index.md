## Introduction

The REST (Rare Event Searches with TPCs) Framework is mainly written in C++ and it is fully integrated with [ROOT](https://root.cern.ch) I/O interface.
REST was born as a collaborative software effort to provide common tools for acquisition, simulation, and data analysis of gaseous Time Projection Chambers (TPCs).
The REST Framework provides 3 interfaces that prototype the use of **event types**, **metadata** and **event processes** through `TRestEvent`, `TRestMetadata` and `TRestEventProcess` abstract class definitions.
Any REST library will implement **specific objects** that inherit from those 3 basic interfaces. 

Different **event processes** can be combined to build complex event processing chains with full traceability. 
The **metadata** objects will allow us to provide input parameters or information to the framework using a XML-like format.
REST integrates a special **metadata** object named `TRestManager` that encapsulates all the required information to launch the processing of a particular data chain.
REST will produce output using ROOT format. Any REST file will always contain a `TRestRun` metadata object.
`TRestRun` is a **metadata** object responsible to encapsulate and give access to all the objects stored inside the REST/ROOT file; 
i.e. the **specific** resulting `TRestEvent` output, the `TRestAnalysisTree`, and any **specific** `TRestMetadata` object used during a processing chain.

The REST Framework provides additionally different interfaces to **browse data**, `TRestBrowser`, and to **plot data**, `TRestAnalysisPlot`, by accessing `TRestEvent` and `TRestAnalysisTree` ROOT-based drawing methods.
Other objects included in the framework will help to add unit definitions, `REST_Units`, define physical constants `REST_Physics`, or provide methods to help on text formatting `TRestStringHelper` or define output styles, `TRestStringOutput`.


[**next**](installing.md)