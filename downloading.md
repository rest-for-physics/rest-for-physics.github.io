---
layout: default
title: Downloading
nav_order: 15
---

These instructions will get you a copy of the project up and running on your local machine in your home directory.
The recommended way to download a copy of REST will be to clone it using the corresponding git command.

### Downloading an official REST release using git

When we download/clone the REST framework repository on our local system, the latest development version will be downloaded.
We can switch and install any official REST release by cloning any particular *git tag*.
We define a REST-for-Physics release as official when the corresponding commit has been tagged using the git system. 
See also the [versioning page](rest-basics/rest-versioning.md).

For example, to create a git branch connected to the REST release v2.2.6, you will do the following.

```
git clone https://github.com/rest-for-physics/framework rest-framework
cd rest-framework
git checkout tags/v2.3.10
git checkout -b official
```

### Downloading official submodules (libraries/projects/examples)

In order to get the full REST-for-Physics functionality, it will be necessary to download/pull few submodules, including libraries, project examples, or packages.

```
python3 pull-submodules --clean
```

You may make sure the change took place by checking the status and commit history.

```
git status
git log
```

### Pulling private project submodules

If you have pulled changes in a particular submodule, or added your own commits, be aware that calling again to `python3 pull-submodules.py` will bring the state of submodules to the official ones at the main repository. REMOVING! any commits you may have at the submodule and that were not pushed to a remote.

If you have access to private repositories, related to projects or experiments inside the REST community you may pull those executing an additional command.

```
python3 pull-submodules.py --lfna (or --sjtu)
```

### Pulling the latest state of each submodule (non-official)

On top of that, you might get the latest state of each submodule by executing

```
python3 pull-submodules.py --lfna --latest
```

But, if you wish to remain at the reference/official release, and get just the latest state from a particular submodule, it is possible to move to the given submodule and checkout its `master` branch.

```
cd source/libraries/xlib
git checkout master
git pull
```

### Excluding submodules

It is also possible to exclude submodules from being pulled by using the `--exclude:` directive and comma separated submodule names.

```
python3 pull-submodules.py --exclude:axionlib,geant4lib,restG4
```

### Recovering a clean state of rest-framework and submodules

If you have added modifications to the rest-framework code and/or submodules. It is possible to fully clean your repository to be an identical copy to the one found at the remote repository.

Notice that executing the following script (placed at the rest-framework root repository) will completely remove any changes or addons you have done at your local installation.

```
python3 pull-submodules.py --clean
```

If there is any untracked content you might still need to `git clean -d -f` to remove untracked items. If any untracked items or modified files are found at your source repository during compilation the `fCleanState` flag at `TRestMetadata` will be set to `OFF`.

[**prev**](index.md)
[**next**](installation.md)
