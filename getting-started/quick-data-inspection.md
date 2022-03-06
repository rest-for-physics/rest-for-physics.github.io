---
layout: default
title: Quick REST file inspection
parent: Getting started
has_children: true
permalink: /getting-started/quick-access
nav_order: 10
---

## Quick REST file inspection
{: .no_toc }

This section shows how to open a ROOT file generated with REST, access the basic metadata information contained within the file, and retrieve the event data and analysis tree observables for a specific entry using its event id or entry number.

The examples shown in this section will use as input a file generated using the example [08.Alphas](https://github.com/rest-for-physics/restG4/tree/master/examples/08.Alphas) implemented inside [restG4](../rest-g4/rest-g4.md). The [README.md](https://github.com/rest-for-physics/restG4/tree/master/examples/08.Alphas/README.md) contains information on how to generate a first dataset. We will need the file generated after executing the `analysis.rml` configuration file. In our case the file name is `Run_g4Analysis_5MeV_1um.root`.

If you use that example make sure you activate the `outputEventStorage` parameter.

```xml
<parameter name="outputEventStorage" value="on" />
```

The different sub-sections will show how to perform those REST file actions with different interfaces, a python environment or script, a C-macro based on ROOT, or opening the file objects using `restRoot` ROOT interface with pre-loaded REST libraries.

{: .no_toc .text-delta }

1. TOC
{:toc}

