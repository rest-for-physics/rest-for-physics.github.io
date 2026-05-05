---
layout: default
title: Quick REST file inspection
parent: Getting started
has_children: true
has_toc: false
permalink: /getting-started/quick-access
nav_order: 10
---

## Quick REST file inspection
{: .no_toc }

This section shows how to open a ROOT file generated with REST, access the basic metadata information contained within the file, and retrieve the event data and analysis tree observables for a specific entry using its event id or entry number.

REST files are ROOT files with a REST structure. They usually contain a [`TRestRun`](https://rest-for-physics.github.io/framework/classTRestRun.html), metadata objects, an event tree, and a [`TRestAnalysisTree`](https://rest-for-physics.github.io/framework/classTRestAnalysisTree.html) with the observables produced during processing. The exact event type and observable names depend on the processing chain that created the file.

The most common first checks are:

- opening the file and printing the run metadata;
- checking the number of entries and the event type stored in the file;
- listing the available analysis observables;
- loading a given entry or REST event ID;
- drawing observables or events to verify the contents of the file.

The pages below show the same kind of file inspection using different interfaces.

## Choosing An Interface

[`restRoot`](quick-data-inspection-restRoot.md) is usually the fastest option for interactive inspection. It opens a ROOT shell with the REST libraries loaded, creates convenient objects such as `run`, `ana_tree`, and `ev`, and allows immediate use of REST drawing methods, ROOT plots, `TBrowser`, and the REST event viewer.

[`C-macros`](quick-data-inspection-Cmacro.md) are useful when the inspection becomes repetitive or when several plots, cuts, or fits need to be produced in a controlled way. They can be executed from `restRoot`, so REST classes are available together with normal ROOT functionality.

[`pyROOT`](quick-data-inspection-pyROOT.md) is useful for scripting, notebooks, integration with external Python tools, and analysis workflows that combine REST data with Python packages.

## Example Input Files

The commands shown in these pages can be used with any REST file. If you do not have one available, the [restG4 08.Alphas example](https://github.com/rest-for-physics/restG4/tree/master/examples/08.Alphas) can be used to generate a small simulation file. After running the `analysis.rml` configuration, the example produces a file such as:

```text
Run_g4Analysis_5MeV_1um.root
```

If the generated file should contain the event data as well as the analysis observables, make sure that event storage is enabled in the processing configuration:

```xml
<parameter name="outputEventStorage" value="on" />
```

Files that only contain an analysis tree are still useful for plotting observables, but they cannot be used to draw individual REST events.

## Next Steps

- [Inspect a file interactively with restRoot](quick-data-inspection-restRoot.md).
- [Inspect and analyze a file with C-macros](quick-data-inspection-Cmacro.md).
- [Inspect and analyze a file with pyROOT](quick-data-inspection-pyROOT.md).
