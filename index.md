---
layout: default
title: Home
nav_order: 10
---

[REST-for-Physics on GitHub](https://github.com/rest-for-physics){: .btn .btn-blue .fs-4 .mb-4 .mb-md-0 }
[Forum](http://rest-forum.unizar.es){: .btn .btn-green .fs-4 .mb-4 .mb-md-0 }
[API documentation](https://sultan.unizar.es/rest){: .btn .btn-purple .fs-4 .mb-4 .mb-md-0 }
[Zenodo](https://doi.org/10.5281/zenodo.4922415){: .btn .btn-blue .fs-4 .mb-4 .mb-md-0 }
[Publication](https://doi.org/10.1016/j.cpc.2021.108281){: .btn .btn-green .fs-4 .mb-4 .mb-md-0 }

---



**These pages are under continuous construction. Some sections need to be documented yet. If you are willing to see any of the sections in these pages to be completed or updated, please, do not hesitate to [create an issue at this documentation repository](https://github.com/rest-for-physics/rest-for-physics.github.io/issues) asking for missing docs. Thanks!**

<p align="center">
<img src="assets/images/RESTlogoFull.png" width="350">
</p>
  
The REST (Rare Event Searches Toolkit) Framework is mainly written in C++ and it is fully integrated with [ROOT](https://root.cern.ch) I/O interface.
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

## License

This project is licensed under the GNU License - see the [LICENSE](assets/LICENCE) file for details

## Acknowledgments

We acknowledge support from the the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme, grant agreement ERC-2017-AdG788781 (IAXO+), and from the Spanish Agencia Estatal de Investigacion under grant FPA2016-76978-C3-1-P

<p align="center">
<img src="assets/images/ResearchLogos.png" width="700">
</p>

![Insitution logos](assets/images/institution_logos.png)



