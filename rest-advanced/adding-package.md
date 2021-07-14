---
layout: default
title: Adding a new package
parent: REST Advanced
nav_order: 8
---

## Adding a new package
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

**TOBE updated**

It is possible to write a code based on REST and compile it. We can make a library which extends 
functionality to REST mainbody, or we can make an executable calling REST classes.

In the directory ./packages/userRESTLibrary, we have a library project which can directly be compiled.

~/REST_v2$ `cd packages/userRESTLibrary`  
~/REST_v2/packages/userRESTLibrary$ `mkdir build`  
~/REST_v2/packages/userRESTLibrary$ `cd build`    
~/REST_v2/packages/userRESTLibrary/build$ `cmake ..`  
~/REST_v2/packages/userRESTLibrary/build$ `make install`  

The default installation path of this project is "userRESTLibrary/lib". If we add this to our 
LD_LIBRARY_PATH:

`export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/REST_v2/packages/userRESTLibrary/lib`

Then REST will load this library at its startup, new classes in it will be available.

To build our own executable based on REST, we can take a look at restG4. To use REST classes we just 
need to include corresponding headers. To use reflection functionalities we need to dynamically load 
REST libraries in the program. For example:

```
//in file test.cxx

#include "TRestRun.h"`  
#include "TRestGas.h"`  

int main(int argc, char *argv[]) {  

	TRestTools::LoadRESTLibrary(true);
	TRestRun *run = new TRestRun();
	run->OpenInputFile("abc.root");
	TRestGas *gas = (TRestGas *)run->GetMetadataClass("TRestGas");
	gas->InitFromRootFile();
	cout<<gas->GetDriftVelocity(70)<<endl;

}  
```

If we remove the 4th line, there will be a warning message from TRestGas telling us REST cannot 
use gas utility. Because the gas library needs to be dynamically loaded.

Note that this source file uses only REST and ROOT6. Such file can be compiled into 
library/executable with a single command:

* library: ``gcc test.cxx -std=c++11 -I`root-config --incdir` -I`rest-config --incdir```   
```rest-config --libs` `root-config --libs` -lGui  -lEve -lGeom -lMathMore -lGdml -lMinuit``  
``-L/usr/lib64 -lstdc++ -shared -fPIC -o test.so``

* executable: ``gcc test.cxx -std=c++11 -I`root-config --incdir` -I`rest-config --incdir```  
```rest-config --libs` `root-config --libs` -lGui  -lEve -lGeom -lMathMore -lGdml -lMinuit``  
``-L/usr/lib64 -lstdc++ -fPIC -o test``
