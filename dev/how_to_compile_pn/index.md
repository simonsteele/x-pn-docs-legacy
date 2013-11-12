---
layout: up
title: How to Compile PN
---
# How to Compile PN

## Requirements:

  * Microsoft Visual Studio 2010
  * [Subversion](http://subversion.tigris.org/)
  * [Git](http://git-scm.com/downloads/guis)
  * [Boost](http://www.boost.org/) library: 1.53

## Pre-requisite Installation

  - Download the boost library (link above)
  - Extract boost somewhere on your HD.  You don't need to build it, since PN will only use the headers (unless you're building PyPN).
  - Add the boost directory to your Visual C++ include directories (Tools|Options|Projects and Solutions|VC++ Directories)
  - Note that you can alternatively add manual include references to each project that needs it if you prefer not to do the above step.
  - Get the SVN version of the WTL: `svn co https://wtl.svn.sourceforge.net/svnroot/wtl/trunk/wtl wtl`
  - Add the wtl/include directory to your Visual C++ include directories, and also your resource include directories:
    - Goto Tools|Options|Projects and Solutions|VC++ Directories
    - For "Show directories for" select "Include Files"
    - Add the WTL directory to the list
    - Click ok
    - Now right-click on the "PN" project in the solution explorer (left side)
    - Goto Properties > Configuration Properties > Resources > General
    - Add the WTL path to the "Additional Include Directories" field
  - Note that you can alternatively add manual include references to each project that needs it if you prefer not to do the above step.
    - // **TimMagee** If that's what you choose to do you also need to add it to the resource includes for **pn**//

## PN Download and Build Steps =====

  - Get the PN code from Google Code: `git clone https://github.com/simonsteele/pn.git`
    - // **TimMagee** Depending on your source control client's setup you may then need to add execute permissions to DLL and EXE files in the bin directory.  You'll know that you do if, when you come to run pn.exe (see below), you get a c0000022 exception.
  - Now go to pnwtl/ and open the "pn.sln" file with Visual Studio.
  - Press F7 or Build > Build Solution and then run PN.exe.

_Please update this if you find any omissions_
