---
layout: default
title: Introduction
nav_order: 10
---

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

## Publications

- PandaX-III: Searching for neutrinoless double beta decay with high pressure 136Xe gas time projection chambers. [X. Chen et al., Science China Physics, Mechanics & Astronomy 60, 061011 (2017)](https://doi.org/10.1007/s11433-017-9028-0) [arXiv:1610.08883](https://arxiv.org/abs/1610.08883).
- Background assessment for the TREX Dark Matter experiment. [Castel, J., Cebrián, S., Coarasa, I. et al. Eur. Phys. J. C 79, 782 (2019)](https://doi.org/10.1140/epjc/s10052-019-7282-6). [arXiv:1812.04519](https://arxiv.org/abs/1812.04519).
- Topological background discrimination in the PandaX-III neutrinoless double beta decay experiment. [J Galan et al 2020 J. Phys. G: Nucl. Part. Phys. 47 045108](https://doi.org/10.1088/1361-6471/ab4dbe). [arxiv:1903.03979]( https://arxiv.org/abs/1903.03979).

## License

This project is licensed under the GNU License - see the [LICENSE](assets/LICENCE) file for details

## Acknowledgments

We acknowledge support from the the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme, grant agreement ERC-2017-AdG788781 (IAXO+), and from the Spanish Agencia Estatal de Investigacion under grant FPA2016-76978-C3-1-P

![Insitution logos](assets/images/institution_logos.png)


[**next**](installing.md)
