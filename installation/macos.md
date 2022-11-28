---
layout: default
title: REST in MacOs
parent: Installation
nav_order: 22
---

## REST in MacOs

Here you will find a quick recipe for a minimal REST build in MacOs.

1. Make sure your default shell is set to bash, use the instructions "From system preferences" from [the following site](https://www.howtogeek.com/444596/how-to-change-the-default-shell-to-bash-in-macos-catalina/).

2. Install XCode found at the AppStore.

3. Install `brew` command tool that can be installed following [these instructions](https://treehouse.github.io/installation-guides/mac/homebrew).

4. Install `cmake` command tool using `brew install cmake`.

5. Clone REST repository using the command: `git clone https://github.com/rest-for-physics/framework rest-framework`. For additional details, downloading a particular release, or pulling REST-for-Physics libraries and packages, check the [download documentation page](https://rest-for-physics.github.io/downloading.html).

6. Install ROOT via any of these options:
    - Execute the command "brew install root"
    - Go inside the rest-framework installation scripts directory: `cd rest-framework/scripts/installation/` and execute `./installROOT.sh`. Then, you need to source the compiled ROOT: `source $HOME/apps/root-6.24.02/install/bin/thisroot.sh`.

7. Now go to the rest-framework directory executing `cd rest-framework`, create a directory `mkdir build` and execute: `cmake -DCMAKE_INSTALL_PREFIX=../install ..`

8. Execute `make -j install`.

9. In order to use that particular REST compilation you need to load it executing: `source ../install/thisREST.sh`.

{: .highlight } 
> For additional details enabling REST libraries and packages check the common instructions found at the [installation pages](https://rest-for-physics.github.io/installation).
