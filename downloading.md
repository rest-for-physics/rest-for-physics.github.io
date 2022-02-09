---
layout: default
title: Downloading
nav_order: 15
---

These instructions will get you a copy of the project on your local machine.
The recommended way to download a copy of REST is to clone it using the git command line.

### Downloading an official REST release using git

When we download (or clone) the framework repository on our local system we will download 
the latest development version.

```
git clone https://github.com/rest-for-physics/framework rest-framework
```

We may switch and install any official REST-for-Physics official release by cloning any particular *git tag*.
A REST-for-Physics release is official when the corresponding commit has been tagged using the git system. 
See also the [versioning page](rest-basics/rest-versioning.md).

For example, to create a git branch connected to the REST release v2.3.10, you will do the following inside the recently created `rest-framework` directory.

```
cd rest-framework
git checkout tags/v2.3.10
```

Then you may create a branch to identify your local state.

```
git checkout -b official
```

### Downloading official submodules (recommended)

In order to get the full REST-for-Physics functionality, it will be necessary to download/pull few submodules, including libraries, project examples, or packages.

```
python3 pull-submodules --clean
```

To make sure that the change took place check the status and commit history.

```
git status
git log
```

The `--clean` flag will assure that the official version of the submodule will be downloaded. If you added changes to any submodule be aware that calling again to `python3 pull-submodules.py --clean` will bring the state of submodules to the official ones at the main repository. REMOVING! any commits you may have at the submodule and that were not pushed to a remote.

### Pulling private project submodules (optional)

If you have access to private repositories, related to projects or experiments inside the REST community you may pull those executing an additional command.

```
python3 pull-submodules.py --lfna (or --sjtu)
```

### Pulling the latest state of each submodule (non-official)

On top of that, you might get the latest state (master branch) of each submodule by executing

```
python3 pull-submodules.py --lfna --latest
```

But, if you wish to remain at the official release, and get just the latest state from a particular submodule, it is possible to move to the given submodule and checkout its `master` branch.

```
cd source/libraries/xlib
git checkout master
git pull
```

### Excluding submodules (advanced)

It is also possible to exclude submodules from being pulled by using the `--exclude:` directive and comma separated submodule names.

```
python3 pull-submodules.py --exclude:axionlib,geant4lib,restG4
```

### Recovering a clean state of rest-framework and submodules (trick)

If you have added modifications to the rest-framework code and/or submodules. It is possible to fully clean your repository to be an identical copy to the one found at the remote repository.

Notice that executing the following script (placed at the rest-framework root repository) will completely remove any changes or addons you have done at your local installation.

```
python3 pull-submodules.py --clean
```

If there is any untracked content you might still need to `git clean -d -f` to remove untracked items. If any untracked items or modified files are found at your source repository during compilation the `fCleanState` flag at `TRestMetadata` will be set to `OFF`.

[**prev**](index.md)
[**next**](installation.md)
