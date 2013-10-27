---
layout: up
title: Building Boost Libs
---

# How to build the Boost libraries that PN uses

Copies of these required libraries are checked in to the source tree, so you should only need to do this to take a new Boost version.

## Building Boost Python for Multiple Python Versions

Create the file user-config.jam in your user directory with this content:

```
using python : 2.6 ;
using python : 2.5 ;
using python : 2.4 ;
```

Since Boost 1.36 the syntax for building the project has changed, and to build staged libs with shared DLLs and shared runtime references you need this fun command:

```
b2 --with-python toolset=msvc-10.0 runtime-link=shared link=shared python=2.4 stage
```

For each version of Python you want to use, build with the correct version in the command and then copy the built .lib and .dll files into the PN lib folder under the correct python version before building again for the next python version.

## Building Boost Test

```
b2 --with-test toolset=msvc-10.0 runtime-link=shared link=shared stage
```

After copying all the libs into the relevant PN lib directory be sure to check them in so others don't need to build them!
