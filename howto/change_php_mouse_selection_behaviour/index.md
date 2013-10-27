---
layout: up
title: Change the PHP Mouse Selection Behaviour
---

# Change the PHP Mouse Selection Behaviour

As of Programmer's Notepad 2.2, double-clicking to select text with the PHP scheme selected includes the $ dollar character, e.g. all of $variable will be selected. This behaviour was requested specifically by many PHP users. 

To change this behaviour open up WebFiles.scheme and find the wordchars attribute in the PHP language, e.g.

```
  wordchars="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789$_-"
```

Remove the $ from the attribute. Also do the same for phpscript.scheme if you use that.