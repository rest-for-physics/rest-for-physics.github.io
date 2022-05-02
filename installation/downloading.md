---
layout: default
title: Downloading
parent: Installation
nav_order: 20
---

### Downloading an official REST release using git

When we download/clone the REST framework repository on our local system, the latest development version will be downloaded.
We can switch and install any specific REST release by cloning any particular *git tag*.

For example, to create a git branch connected to the REST release v2.2.6, you will do the following.

```
git clone https://github.com/rest-for-physics/framework rest-framework
cd rest-framework
git checkout tags/v2.3.6
git checkout -b official
python3 pull-submodules --clean
```

You may make sure the change took place by checking the status and commit history.

```
git status
git log
```

