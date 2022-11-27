---
layout: default
title: REST in Windows
parent: Installation
nav_order: 21
---

## REST in Windows

We recommend a UNIX environment for development of REST-for-Physics libraries since most of the active developers maintain and contribute to REST using UNIX development environments. Still it exists the possibility to run and use REST inside Microsoft Windows.

Here you will find a quick recipe to get REST up and running in Windows. At least this is what I had to do to get it working.

1. Install Microsoft Visual Studio 2022 (C++ developper tools)

2. Execute Visual Studio and select to clone the REST-for-physics public [framework repository](https://github.com/rest-for-physics/framework.git).

3. Open a terminal and place yourself at the directory where the repository was downloaded.

4. Install python (You will be prompted with an installation window as soon as you try to execute the command).

5. Install `git` for Windows and add the binary PATH manually, see the following [link](https://stackoverflow.com/questions/4492979/git-is-not-recognized-as-an-internal-or-external-command).

6. [Download ROOT 6.26.02 binaries](https://root.cern/releases/release-62602/) for Visual Studio 2022 and 64 bits. Unzip it to a directory of your convenience (in the following this directory will be referred as `ROOT_PATH`).

7. Inside Visual Studio enter into the top menu `Project->CMake Settings` and add the variable `ROOT_DIR` pointing to your downloaded ROOT binary path `ROOT_PATH/cmake`

8. In the same configuration settings, at the "CMake command arguments", add `-CMAKE_MODULE_PATH=ROOT_PATH/cmake`, where `ROOT_PATH` is the directory where you extracted the ROOT binaries.

9. Add any REST libraries you are willing to use in the same line, e.g. `-DRESTLIB_AXION=ON` or `-DREST_ALL_LIBS=ON`.

10. To re-run cmake just go to "Project->Configure cache", and to build press F7.


{: .note } 
The [PR#231](https://github.com/rest-for-physics/framework/pull/231) inside the framework might give some light in case of problems. If you find issues and solutions following the previous instructions do not hesitate to update them.

{: .warning } 
> The [PR#231](https://github.com/rest-for-physics/framework/pull/231) inside the framework might give some light in case of problems. If you find issues and solutions following the previous instructions do not hesitate to update them.
