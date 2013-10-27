---
layout: up
title: Text Clip Variables
---

# Text Clip Variables

Programmer's Notepad supports the following built-in variables when using Text Clips:

| Variable  | Description |
| --- | --- |
| PN_FILENAME                | The file part of the current filename.     |
| PN_FILEDIRECTORY           | The directory part of the current filename. |
| PN_FILENAMENOEXT           | The file part of the current filename with no extension.     |
| PN_FILEPATH                | The full current filename |
| PN_CURRENTLINE             | Current line number |
| PN_CURRENTCOLUMN           | Current column number |
| PN_CURRENTWORD             | Current word |
| PN_CURRENTLINETEXT         | Current line text |
| PN_SELECTEDTEXT            | Selected text |
| PN_CURRENTPROJECTFILE      | Current project file |
| PN_CURRENTPROJECTGROUPFILE | Current project group file |
| PN_PROJECTPATH             | Current project path |
| PN_PROJECTGROUPPATH        | Current project group path |
| PN_PROJECTPROP:<PROP>      | Property from the current project, using group.category.value syntax to specify the property to retrieve. e.g. ${ProjectProp:test.general.outputdir} |
| PN_FILEPROP:<PROP>         | Property from the current file, using the same syntax as ProjectProp |
| PN_PROJECTNAME             | Current project name |
| PN_PROJECTGROUPNAME        | Current project group name |
| PN_PNPATH                  | Path where Programmer's Notepad is installed |

Programmer's Notepad also supports most TextMate variables, e.g. TM_FILEPATH.