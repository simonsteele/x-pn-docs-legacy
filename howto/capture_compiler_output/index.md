---
layout: up
title: Capture Compiler Output
---

# Capture compiler output

This mini-guide will show you how to capture the output of a tool such as a compiler or linker.

## Basic Settings

Open up the Options dialog from the Tools menu, and select the Tools tab. The drop-down box at the top of the tools page allows you to select a scheme that the tool should be available with. You can also select "(None - Global Tools)" if you wish the tool to be available everywhere in PN.

Click "Add" to add a new tool. A new window will pop up allowing you to enter the details of the tool. The first thing to choose is the name of the tool, something like "Compile".

Next up is the "Command" for the tool, here you select the program that the tool will run, for example "cl.exe". Don't put any parameters here, they go further down!

The "Folder" field allows you to set the current directory for when the tool runs. It is most often a good idea to set this to the folder of the current file you are working on, so put in %d.

Put the parameters for your tool in the "Parameters" field! You can use any of the shortcuts shown in the "Special Symbols" box at the bottom of the page, for example ''%f'' for the filename of the current file or ''$(ProjectPath)'' for the path to the current project file. If you just want to pass in the full current filename, you should use ''"%d%f"''

You can choose a shortcut key to use to run your tool, just put your cursor in the shorcut box and press the key combination you want, e.g. or .

It's often useful to save the current or all files before running a tool. The Save option allows you to select what files should be saved when running the tool.

## Capturing the tool output

Select the second tab, titled "Console I/O". This tab allows you define how PN will interact with your tool.

To capture the output from your tool, simply select the "Capture Output?" option. You can then choose whether you want the output to be placed in the global output window (the main, docking output window) or in an output pane attached to the current document. Finally, you can choose whether to clear the output window before running the tool.

PN allows you to click on errors and warnings in your tool output and jump to the relevant parts of source file. It does this by parsing the output from the tool and finding text such as '' "myfile.cpp:21: Error..." ''. PN has built in support for several tools, to try this support leave the "Use the built-in error parser" option checked and run your tool. If you find that PN does not pick up errors from your tool then you will need a custom pattern. To see how to use custom patterns to match errors and warnings see here.
