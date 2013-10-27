---
layout: up
title: Magic Folders
---

# Magic Folders
A Magic Folder is a project folder that automatically includes files and folders from the computer's file system. Magic Folder contents are automatically updated when the files on the disk change.

You can use a filter to select which files are visible in a Magic folder; for example, object files can be hidden in a source code folder.

## Create a magic folder
To create a Magic Folder you must first create a new [Project]({{ site.url }}/projects/introduction) or open an existing one.

  - Right-click on the project's icon in the Project window and select Add Magic Folder from the popup menu.
  - In the Magic Folder wizard that opens browse and select the file system folder that you want to add to your project and click Next.
  - In the next step of the wizard select the types of files that you want to include in the folder.\ \ For example, if you're working on a Java project you might specify ".java;.properties;.xml" to include only Java source and configuration files.\ \ You can also specify a list of directory names that you want to exclude from the Magic Folder. This is useful to exclude folders created by tools, such as the "CVS" folder created by CVS version control.
  - Finally, click Finish and all of the files and sub-folders matching the filter patterns are automatically added to the Magic folder.

_Tip_: You can modify the filters later in the Magic Folder's properties. Right-click the folder's icon in the Project window and select Properties from the popup menu.
