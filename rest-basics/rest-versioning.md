---
layout: default
title: Code versioning
parent: REST Basics
nav_order: 12
---

## Code versioning
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The REST versioning system stamps the data processed and generated with REST with different metadata members (`fVersion`, `fCommit`, `fCleanState`, `fOfficialRelease`) defined as private members inside [TRestMetadata](https://sultan.unizar.es/rest/classTRestMetadata.html). These members will be initialized for any specific metadata class with unique version numbers, and it will allow to identify the code used to generate the data. However, **reproducibility** will be guaranteed only if the compilation was generated with a clean state, i.e. `TRestMetadata::fCleanState=true`.

The stamped version number in the file might serve as a solution to reproduce or recover previous results which may show discrepancies with future versions. The **version number shall be provided together with published or internal results**. If we have access to a particular data file we will always be able to identify the official version of the code used to generate those results.

The version number of an official REST code release is built using 3 digits (2.X.Y, being X and Y the major and minor revision numbers) that identify major or measurable steps on the evolution of the code. This number is registered at any metadata class written to disk. It must be noticed that a REST data file processed at different stages may contain objects with different version numbers, if processed with different REST versions on different stages. The [TRestRun](https://sultan.unizar.es/rest/classTRestRun.html) object available inside the data file will always be stamped with the code version of the REST release used in the last processing.

Further details on the versioning strategy of REST will be found at our [contribution guide](https://github.com/rest-for-physics/framework/blob/master/CONTRIBUTING.md). A step by step guide to generate a new REST official release will be found [here](../rest-advanced/new-release.md). A summary of the changes introduced at each code release will be found at the git repository, [tags section](https://github.com/rest-for-physics/framework/releases).

## REST libraries versioning

The libraries in REST are connected to the main framework using `git submodules`. An official REST release is directly connected to a submodule commit id, being that submodule/library commit id the official REST library version. Therefore, the official versioning of libraries is controlled by the main framework. The main framework repository will keep a link to the exact commit for each submodule that will be downloaded with the official version.

Eventhough **the versioning is centralized and managed by the framework**, each library defines an internal version number so that users may identify changes happenning on a particular library through the release notes, since those changes are connected again with the tag release of the git repository. Changes will be described at the tags section, as for example in the [rawlib releases description](https://github.com/rest-for-physics/rawlib/releases).

When working at our local copy we might wish to recover the official state of all submodules in our source code directory. Then, we may use the `pull-submodules.py` command to remove any undesired local modification introduced at the submodules, or libraries, and **place ourselves at the official commit** for each submodule:

```
python3 pull-submodules.py --clean
```

In case we wish to **switch to the development version of submodules**, we may get the latest commit at any submodule master branch by executing:

```
python3 pull-submodules.py --latest
```

This will place each library at the latest commit found at each `master` branch of each library.

Instruction to generate a new official library version will be found at each library contribution guide, for example, at the [rawlib contribution guide](https://github.com/rest-for-physics/rawlib/blob/master/CONTRIBUTING.md).

## Internal class versioning (ROOT Schema evolution)

Since version 2.2.1, the REST-for-Physics framework is properly implementing automatic schema evolution feature of ROOT. In brief, when class members at a REST class are modified, the class version number should be updated manually at the `ClassDef` call at the class header, where the version number of the class is defined. For example, the following line inside [TRestAxionMagneticField](https://sultan.unizar.es/rest/TRestAxionMagneticField_8h_source.html) defines the class version 3 for that specific class, line found at the end of the file.

```
ClassDef(TRestAxionMagneticField, 3);
```

When **modifying the data members** of the class the developer **should increase the version number** given at this call. That action will help ROOT to read previous versions of the class written to disk, and it will **guarantee backwards compatibility**.

