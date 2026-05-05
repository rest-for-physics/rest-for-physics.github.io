---
layout: default
title: Filtering events
parent: Getting started
nav_order: 20
---

## Filtering events using analysis observables conditions
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Filtering usually starts from the observables stored in the [`TRestAnalysisTree`](https://rest-for-physics.github.io/framework/classTRestAnalysisTree.html) ([Sultan mirror](https://sultan.unizar.es/rest/classTRestAnalysisTree.html)). These observables are scalar quantities produced by REST processes, such as energies, positions, number of hits, time information, topology variables, or detector-specific analysis results.

The event object and the analysis tree are connected entry by entry. This means that an observable cut can be used to find interesting entries, and the corresponding REST event can then be loaded, printed, or drawn.

## Choosing A Filtering Method

For quick checks, use the analysis tree directly from `restRoot`:

```bash
restRoot myFile.root
```

Inside the `restRoot` session:

```cpp
ana_tree->PrintObservables();
ana_tree->Draw("tckAna_MaxTrackEnergy");
ana_tree->Draw("tckAna_MaxTrackEnergy", "tckAna_MaxTrackEnergy>2000");
```

For REST-aware event selection, use the helper methods provided by `TRestRun`:

```cpp
auto entries = run->GetEventEntriesWithConditions("tckAna_MaxTrackEnergy>2000");
auto ids = run->GetEventIdsWithConditions("tckAna_MaxTrackEnergy>2000");
```

For longer analyses, ROOT `RDataFrame` is often convenient:

```cpp
ROOT::RDataFrame df("AnalysisTree", "myFile.root");
auto selected = df.Filter("tckAna_MaxTrackEnergy>2000");
```

For processing pipelines, use a REST event-selection process in the RML configuration.

## Finding Observable Names

Before writing a cut, inspect the observables available in the file:

```bash
restRoot myFile.root
```

Inside the `restRoot` session:

```cpp
ana_tree->PrintObservables();
```

or from a `TRestRun` object:

```cpp
run->PrintObservables();
```

Observable names depend on the processes used to create the file. A track file may contain observables such as `tckAna_MaxTrackEnergy`, while a detector-hit analysis may contain position observables such as `hitsAna_xMean` and `hitsAna_yMean`.

## Filtering With ROOT Cuts

Since `TRestAnalysisTree` inherits from ROOT `TTree`, standard ROOT selection strings can be used in `Draw`:

```cpp
ana_tree->Draw("tckAna_MaxTrackEnergy");
ana_tree->Draw("tckAna_MaxTrackEnergy", "tckAna_MaxTrackEnergy>2000");
ana_tree->Draw("hitsAna_yMean:hitsAna_xMean", "hitsAna_nHits>20", "colz");
```

The first argument defines what is drawn. The second argument is the selection. The third argument contains drawing options, such as `colz` for a two-dimensional color map.

This is useful for fast visual checks, but it does not by itself create a new REST file or remove events from the input file.

## Getting Entries From REST Conditions

`TRestRun` provides helper methods to translate observable conditions into REST entries or event IDs:

```cpp
auto entries = run->GetEventEntriesWithConditions("tckAna_MaxTrackEnergy>2000");
auto ids = run->GetEventIdsWithConditions("tckAna_MaxTrackEnergy>2000");
```

Multiple conditions can be combined with `&&`:

```cpp
auto entries = run->GetEventEntriesWithConditions("tckAna_MaxTrackEnergy>2000&&tckAna_TotalTrackLength<50");
```

The supported comparison operators for these `TRestRun` helper methods are:

```text
==  =  >  >=  <  <=
```

When using these REST condition helpers, write the condition without spaces around the operator:

```cpp
"tckAna_MaxTrackEnergy>2000"
```

This avoids ambiguity when parsing the observable name.

The optional arguments can be used to start searching from a given entry and limit the number of selected events:

```cpp
auto entries = run->GetEventEntriesWithConditions("tckAna_MaxTrackEnergy>2000", 0, 10);
```

## Loading Selected Events

Once the selected entries are known, use `TRestRun::GetEntry` to load the corresponding event and analysis-tree entry:

```cpp
auto entries = run->GetEventEntriesWithConditions("tckAna_MaxTrackEnergy>2000", 0, 10);

for (auto entry : entries) {
    run->GetEntry(entry);
    ev->PrintEvent();
    ev->DrawEvent();
}
```

The pointer `ev` is created automatically when the file is opened with `restRoot`. It points to the current REST event, so it is updated after each call to `run->GetEntry(entry)`.

If the event IDs are more useful than the entry numbers:

```cpp
auto ids = run->GetEventIdsWithConditions("tckAna_MaxTrackEnergy>2000", 0, 10);

for (auto id : ids) {
    run->GetEventWithID(id);
    ev->PrintEvent();
}
```

Entry numbers are positions in the file. Event IDs are identifiers stored in the event metadata, and they are not necessarily equal to the entry number.

## Getting The Next Matching Event

For interactive inspection, `TRestRun::GetNextEventWithConditions` can be used to step through matching events one by one:

```cpp
run->GetNextEventWithConditions("tckAna_MaxTrackEnergy>2000");
ev->PrintEvent();
ev->DrawEvent();
```

Calling the method again advances to the next event that satisfies the condition.

## Checking A Cut On The Current Entry

The analysis tree can evaluate a condition on the current entry:

```cpp
run->GetEntry(10);

if (ana_tree->EvaluateCuts("tckAna_MaxTrackEnergy>2000&&tckAna_TotalTrackLength<50")) {
    ev->PrintEvent();
}
```

This is useful inside short loops or debugging macros when the event has already been loaded.

`TRestAnalysisTree::EvaluateCuts` also accepts `!=` conditions:

```cpp
ana_tree->EvaluateCuts("tckAna_MaxTrackEnergy!=0");
```

## Filtering With RDataFrame

ROOT `RDataFrame` is useful when the goal is to produce histograms, counts, or derived columns from selected entries:

```cpp
ROOT::RDataFrame df("AnalysisTree", "myFile.root");

auto selected = df.Filter("tckAna_MaxTrackEnergy>2000");
auto nSelected = selected.Count();
auto hEnergy = selected.Histo1D({"hEnergy", "Selected energy", 100, 0, 10000}, "tckAna_MaxTrackEnergy");

std::cout << "Selected entries: " << *nSelected << std::endl;
hEnergy->Draw();
```

Several conditions can be chained:

```cpp
auto selected = df.Filter("tckAna_MaxTrackEnergy>2000")
                  .Filter("tckAna_TotalTrackLength<50");
```

This approach is well suited for analysis plots. If the corresponding REST event object is needed for drawing or printing, use the selected entry numbers with `TRestRun` as shown above.

## Filtering During REST Processing

When the goal is to keep or reject events during a REST processing chain, use [`TRestEventSelectionProcess`](https://rest-for-physics.github.io/framework/classTRestEventSelectionProcess.html) ([Sultan mirror](https://sultan.unizar.es/rest/classTRestEventSelectionProcess.html)) in the RML configuration.

For example:

```xml
<addProcess type="TRestEventSelectionProcess" name="evSelection"
    conditions="tckAna_MaxTrackEnergy>2000"
    value="ON" verboseLevel="info" />
```

The process can also read event IDs from a text file, or read event IDs from another ROOT file using observable conditions. In a processing chain, make sure that the observables used in the condition have already been produced by earlier processes.

## Common Problems

If a condition returns no entries, first check that the observable exists:

```cpp
ana_tree->PrintObservables();
```

If the observable exists but the selection still fails, draw the distribution before choosing a threshold:

```cpp
ana_tree->Draw("tckAna_MaxTrackEnergy");
```

If a selected entry does not show the expected event, check whether you are using entry numbers or event IDs. Use `GetEntry(entry)` for entry numbers and `GetEventWithID(id)` for event IDs.

If the file contains only an analysis tree and no stored event data, observable filtering will still work, but individual REST events cannot be drawn.
