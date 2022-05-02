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

The `--clean` argument given to `pull-submodules.py` will download the official REST-for-Physics submodules,
and not the latest commit.

## Getting the latest REST development version

We recommend using REST-for-Physics releases for official data production and data analysis. Still,
it is a good idea to get the latest version for debugging and developing any required features your analysis
may require.

In order to get the latest version you should make sure your local repository is placed at the master branch.

```
cd ~/rest-framework
git checkout master
git pull
```

The previous commands should take you to the latest commit in the master branch at the rest-for-physics/framework
repository. Double-check using `git status` and `git log` commands.

Now, in order to get the latest master branch from 

```
python3 pull-submodules --latest
```

Additionally, if you want to pull the latest development from a particular branch name, you may indicate it at
the `--latest` argument.

```
python3 pull-submodules --latest:user_issue_13
```

The previous command will pull all branches named `user_issue_13` from the submodules. If the submodule
repository does not contain a branch named `user_issue_13`, then the `master` branch will be downloaded.

Of course, we will also be able to attempt to do it manually for each submodule.

```
cd source/libraries/detector
git fetch
git checkout user_issue_13
```
