---
layout: up
title: Install PyPN
---
# Installation instructions for the PyPN extension

## Download & extract extension files

The files are available at http://code.google.com/p/pnotepad/downloads/list.

Be sure to get the version that matches the Python version you use. Using a wrong version will cause problems. If you have multiple versions of Python installed, running "python" in dos prompt / powershell window will run the default version used by Windows. Observe what version gets started; that is the version you'll need PyPN for.

Extract the files into the PN install directory; typically this is "C:\Program Files\Programmer's Notepad\".

## Register the extension

After this, run `pn.exe --findexts`. That will tell PN to search for extensions and register them. 

_Note_ : this will have the effect of adding `<extension path="pypn.dll"></extension>` to config.xml

If everything went as it should, you can then open the Scripts window from the menu (View -> Windows -> Scripts or just press Alt+F10). Try doubleclicking on scripts and see what happens.
