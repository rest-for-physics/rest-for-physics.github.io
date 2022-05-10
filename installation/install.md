---
layout: default
title: Install
parent: Installation
nav_order: 21
---

### Installing

After ROOT6 has been installed in the system, the compilation of REST should be straight forward. 
Note that it is recommended to compile REST using the same version of g++ compiler used to compile ROOT.

Go to the root directory of your local REST repository, lets name it here `REST_SOURCE_PATH` and execute the following commands.

```
cd ~/rest-framework
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=../install/master/ 
make -j4 install
```

After all the compilation and installation process ends, you will end up with an installed REST version at `~/rest-framework/install/master/`.

Execute the following command to configure your `.bashrc` to load REST in your system environment each time you open a new shell terminal.

 ```
 echo "source ~/rest-framework/install/master/thisREST.sh" >> .bashrc
 ```
