---
layout: up
title: Getting Started with Projects
---

# Getting Started With Projects
Projects are used to organize and work with groups of related files. For example, the source code files of a program or library that you're working on.

## The Project Window
You can manage projects using the tree view in the Projects window. To view the Projects window select the View > Windows and ensure that the Project item is checked. The window can be docked by dragging the title bar to an edge of the Programmer's Notepad window.

## Create a project
To create a new project select File > New > Project. In the dialog that opens enter a name for the project. Enter the folder name or browse to the folder where you wish to save the project file. Project files have the name of the project followed by a .pnprj extension, e.g. MyProject.pnprj.

_Note_: The project file doesn't have to be stored in the same place as the files included in the project.

## Magic Folders
A simple way to add files to a project is to use a [Magic Folder]({{ site.url }}/projects/magic_folder/). A Magic Folder automatically includes selected files and sub-folders of a folder on the computer's file system. See the [Magic Folder]({{ site.url }}/projects/magic_folder/) page to learn how to use them.

## Adding files and folders to a project
You can add files and folders to a project manually. This allows you to organize files differently from the file system.

To create a new folder right-click on the project or one of its folders and select Add New Folder in the popup menu. Enter a name for the new folder. You can add as many folders as you like and nest them too. Note that folders created in this way exist only within the project, they are not present in the file system.

To add existing files to a project right-click on a project or folder in the Projects window. Select Add Files from the popup menu and in the dialog that opens browse and select the file or files that you want to add. Finally click Open to add them.

To create a new file within a project first create the new file (File > New). When the file is open in the editor window right-click on its content area. Point to Add to Project in the popup menu and click the project you want to add the file to.

Note: You can move files between folders or projects by dragging and dropping them.

## Open an existing project
To open an existing project select File > Open project(s)... from the menu. Browse to your project file and select it in the file dialog that opens.

## Working with Project Tools
You can set up external tools for projects. For example, you can execute a build of your source code or run unit tests.
