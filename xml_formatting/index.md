---
layout: up
title: XML Formatting Tools
---
# XML Formatting Tools

## HTML Tidy

[HTML Tidy](http://tidy.sourceforge.net/) is useful for reformatting XML, just configure it like this:

**Name**: Format
**Command**: `tidy.exe`
**Folder**: `%d`
**Parameters**: `-iqm -xml -newline CRLF --tab-size 4 "%f"`
**Save**: Current File
[x] This tool will modify the current file

This sets up tidy to format the current file (saving any changes you have first). It is possible to configure this as a filter instead (so that it just modifies text without touching the file) but this has some encoding change problems so is not detailed here.
