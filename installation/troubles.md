---
layout: default
title: Trouble shooting
parent: Installation
nav_order: 27
---

## Trouble shooting

### Garfield not found

During cmake, sometimes it says cannot find Garfield. If you are not necessary with drift speed
and diffusion calculation functionality, you can turn it off: `cmake .. -DREST_GARFIELD=OFF`.

To use Garfield, one must set env "GARFIELD_HOME", "HEED_DATABASE" in the bash. He also needs
to add Garfield's library dir to "LD_LIBRARY_PATH". The garfield library "libGarfield.so" must 
be placed in "$GARFIELD_HOME/lib", and the garfield headers must be placed in "$GARFIELD_HOME/Include".
Garfield is also based on ROOT, and it must be compiled with same ROOT for REST.

Take a look at `installGarfield.sh` for more details.

### cannot find -lGeom, -lGdml, -lSpectrum, -lEve, -lRGL

During compilation, if it reports error "/usr/bin/ld: cannot find -lXXX" of **more than five 
libraries**, this means your ROOT installation is incomplete. It is most possible that you 
are using the OS provided ROOT distribution, which lackes several libraries. Check the 
ROOT installaion with `root-config --libdir`. If the ROOT library directory is /usr/lib64/root, 
then it is the case. You need to manually install ROOT. If not, check also the output of
`cmake` command if it is using ROOT in the unwanted path. If so, source the correct thisROOT.sh,
clear the build dir, and re-run cmake and make.

### cannot find -lGdml

During compilation, if it reports error "/usr/bin/ld: cannot find -lXXX" **including Gdml 
library**, this means your ROOT installation is incomplete. When installing ROOT, you 
must turn on the compilation flag for ROOT to generate gdml library, as in `installROOT.sh`: 
`cmake ../source -Dgdml=ON -DCMAKE_INSTALL_PREFIX=${ROOT_DIR}/install`

### cannot find -lEve, -lRGL

During compilation, if it reports error "/usr/bin/ld: cannot find -lXXX" of **those two
libraries**, this means ROOT really lacks them. Sometimes the manual installed ROOT won't compile
those two libraries because the openGL libraries are not installed in the system. You may need 
to install at least mesa-libGL-devel and glew-devel (and/or xlibmesa-glu-dev and libglew1.5-dev) 
and re-install ROOT. Another solution is to disable eve libraries dependence in REST, by adding 
cmake flags like: `cmake .. -DREST_EVE=OFF`

### error: set was not declared in this scope

In the some releases of gcc, header <set> is added through a different include chain, and must
be manually added. Since REST 2.2.19 we fixed this problem. One can update the REST version or
switch to a different gcc version.

### libtbb.so.2, needed by XXX/libImt.so, not found; undefined reference to `TParticle::Sizeof3D() const'

This happens when one wants to install REST with `sudo make install`. It will together report many 
similar messages. This is because the `LD_LIBRARY_PATH` is cleared when you temporary 
switched to administrator user. The solutions is: 

1. login as administrator to do all the operation. Remember to source the needed files before 
opeartion.

2. run `make` first. After compilation, run `sudo make install` to install.

### undefined symbol XXX

If this happens during installation, this may be a bug of REST code. Contact the developers

If this happens when launching restManager, this may be a problem of ROOT libraries. 
We found that in some platform the ROOT binary are using unmatched version of interface of some system library. 
Try to complie ROOT manually, or change another ROOT binary distribution.
