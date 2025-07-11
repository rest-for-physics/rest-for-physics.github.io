---
layout: default
title: Physics units
parent: REST Basics
nav_order: 30
---

## Units
{: .no_toc }

### Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

### Units in REST

The units inside REST-for-Physics are available in the namespace `REST_Units`, a namespace defines a set of variables and methods that are accessible under that name when invoking `REST_Units::`.

Inside a `restRoot` session we can use the namespace to identify which public data members and methods are available to us. Now, just open a new `restRoot` session and checkout what it is inside the namespace.

```c++
restRoot
root [0] REST_Units:: [TAB]
```

In particular, you will be able to identify few of the REST basic units defined inside.

You may check now the scaling value associated to each unit.

```c++
root [0] REST_Units::degrees [ENTER]
(const double) 57.295780
```

Where the returned value corresponds to the degrees in one radian. The units inside REST work in a way that we can access the scaling factor by invoking `units("UNIT_STRING")`, which will allow us to make unit conversions inside our code.

For example, if we execute,

```c++
root [1] 1. * units("cm")
(double) 0.10000000
```

the result will be the number of `cm` inside 1 `mm` which is the default unit inside REST. We can also combine different units to achieve more complex results, such as:

```c++
root [2] 1. * units("g/cm^3")
(double) 1000000.0
```

If we know the value the units for the value we are providing, and this is not given in the default units, we will then need to write a more generic expression:

```c++
root [3] 180. * units("rads")/units("degrees")
(double) 3.1415927
```

REMEMBER: All these lines of code that you are using inside your `restRoot` session can be used inside a C++ compiled code, a C-macro, or even a python script that imports the REST-for-Physics libraries.

### TRestPhysics

In the same way as units, REST-for-Physics provides access to its own physics constant definitions and geometry methods commonly exploited in particle tracking.

Check inside the `REST_Physics` namespace to see the available constants and print out their value.

```c++
root [4] REST_Physics:: [TAB]

root [5] REST_Physics::lightSpeed
(const double) 2.9979246e+08
```

### Additional Resources

- See day2/session1 of the [rest-school github repository]{https://github.com/rest-for-physics/rest-school/tree/master/day2/session1} to access the codes.
