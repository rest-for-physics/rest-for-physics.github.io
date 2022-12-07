---
layout: default
title: Generating a new official release
parent: REST Advanced
nav_order: 9
---

## Generating a new official release
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

A new official release can be generated at any time in order to include the latest development updates that have been already peer reviewed, approved and merged to the `master` branch. It is important to emphasize that generating a new code release it is "harmless" in the sense that all changes to the code should have been already approved by other REST community members. The new version release update itself will be subject to peer-review and it will require the approval of at least 2 REST community members. Said that, any member of REST-for-Physics at GitHub is allowed to generate a new version release.

The motivation to generate a new code release may be mainly due to one of the following reasons:

1. We require to produce or process official data so that it will be properly stamped and stored as an official REST dataset.
2. The number of changes already included start to be considerable and it would be good to assimilate or summarize the changes.
3. A critical change is introduced affecting the results of a particular data processing chain. E.g. a bug fixed.

The generation of a new version release is a 2-steps process. First we need to identify relevant changes or versions at libraries or packages that we want to be available at the new official release. After any required submodules have been updated at the main framework repository, we will proceed to generate the official version of REST. Those 2-steps will be detailed on the following sections.

## Generating a new REST library version release

Libraries are centrally managed by the REST framework versioning system (see also the basic [versioning guide](../rest-basics/rest-versioning.md)). However, each library is associated to an independent version number so that users identify promptly any changes related to that particular library. On top of that, a new release allows to summarize the relevant changes at each library version. And, at the same time, the version number evolution gives a hint on the activity intensity on a particular library. 

Therefore, if we want to introduce the latest commits of a particular library into the REST official release, it is better to make sure that those latest updates are released and included inside a version number.

To make sure, we check the latest commit where the `CMakeLists.txt` file was updated to increase the library version number, and we identify any future commits after that last version was fixed. If we find commits or changes introduced after the last version was fixed, then we can create a new version release.

### Creating a new branch to update the library version release
We will need to create a new branch where to push the new version update, making sure the new branch inherits from the latest master. We will use the [rawlib](https://github.com/rest-for-physics/rawlib) to illustrate the code examples, where we assume the [framework](https://github.com/rest-for-physics/framework) repository has been cloned to the local `rest-framework` directory.

```
cd rest-framework/source/libraries/rawlib
git fetch
git checkout master
git pull
git checkout -b release_v1.4
```

The previous commands will create a new branch named `release_v1.4` that refers to the new `v1.4` release we are producing. In order to update any library, just modify the following line at the `CMakeLists.txt` file,

```
set( LibraryVersion "1.3" )
```

and increase the version number to `1.4`

```
set( LibraryVersion "1.4" )
```

Then, commit the changes, create a new tag and push to the repository

```
git add CMakeLists.txt
git commit -m "Fixing release 1.4"
git push
```

Then, perform the following actions at the GitHub repository site.

### Merge the new library into master and tag it
Go to the corresponding library GitHub page, and create a new pull-request (PR) to merge the new branch `release_v1.4` into master.

You must assign a member of REST as a reviewer inside the generated PR. The reviewer will approve the new version generation. The reviewer might be any member, but you might select any available member at the `library_dev` team, selecting the group `rest-for-physics/library_dev`. 

Once you merge the PR to the `master` branch, place yourself at the local `master` branch, pull the latest (recent merge) changes, and tag it.

```
git checkout master
git pull
git tag -a v1.4 -m "v1.4"
git push --tags
```

### Create release notes for the new library. 
Go to the tags section inside GitHub, and press edit at the recent tag just created, inserting few bullets creating a list summarizing the changes since the last version. Those points should give an overview, or meaningfull representation, of the new commits.

Repeat this step for any library that you want to be updated at the official REST release.

It must be noted then that library versions might be updated at any time, creating new version numbers with small updates. Then, the new library version will only be part of the official REST framework release if the corresponding library submodule reference is updated, as described in the following section.

Do not hesitate to check the [contribution guide](https://github.com/rest-for-physics/rawlib/blob/master/CONTRIBUTING.md) at each library for further details.

## Submitting a new REST official release

First of all we will need to create a dedicated branch so that we will be able to push it later to the repository. Make sure you are at the latest master inside the `rest-framework` directory by executing the following commands

```
cd rest-framework
git fetch
git checkout master
git pull
git checkout -b release_v2.3.9
```

Then, we add as a commit the latest submodule reference we want to include in the new official release we are preparing, double-checking that the library is at the latest state we want to include.

```
cd rest-framework/source/libraries/rawlib/
git fetch
git checkout master
git pull
git log
```

The last commit at the output of `git log` should correspond with the library commit we want to make official. I.e. in our example the last commit should be "Fixing release 1.4". Then we proceed to update the submodule at the main repository.

```
cd ../../../
git add source/libraries/raw
git commit -m "Updating rawlib to version 1.4"
git push
```

We repeat this process for any libraries/submodules we want to update at the official release.

Then, we need to produce a new `TRestVersion.h` header to create the new REST official release. Just go to the `scripts` directory, and execute:

```
cd rest-framework/scripts
./generateVersionHeader.py
```

The execution generates a new `TRestVersion.h` header that we will use to update the REST version release header. This time follow the instructions given on the screen, by simply copy/pasting the commands starting by `----->`.

Then, at the GitHub site follow the same steps as for generating a library release:

* Go to the corresponding [framework GitHub page](https://github.com/rest-for-physics/framework), and create a new pull-request (PR) to merge the new branch `release_v2.3.9` into master.
* Go to the tags section, and press edit at the recent tag just created, inserting few bullets creating a list summarizing the changes since the last version. Those points should give an overview, or meaningfull representation, of the new commits inside the framework, together with the versions and links to the library or package submodules updated.

Do not hesitate to check the framework [contribution guide](https://github.com/rest-for-physics/framework/blob/master/CONTRIBUTING.md) for further details.
