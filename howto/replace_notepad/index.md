---
layout: up
title: Replace Notepad
---

# Replacing Notepad with PN via Image File Execution Options

This technique involves using a set of registry settings called Image File Execution Options. These settings are used to make Windows run a debugger automatically when a program is launched. We can piggyback on this to run PN instead of notepad when it’s launched.

## Steps

  - Create this Registry Key: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe`
  - Add a string value called “Debugger”, set the value to: `c:\Program Files\Programmer's Notepad\pn.exe --allowmulti -z`

The –allowmulti deals with the second problem listed above, making sure that even if PN is already running a new instance is started – this means that the calling process can wait for PN to exit. The –z tells PN to ignore the next parameter, which when using Image File Execution Options is the process name that we’re replacing – the full path to notepad.exe in this case.

Note that if you’re on 64-bit Windows you’ll need the following Registry Key instead:

```
HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe
```