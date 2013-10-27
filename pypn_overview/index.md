---
layout: up
title: PyPN Overview
---
# PyPN Overview

PyPN is an extension that allows you to use Python to script almost anything in PN.  This can be simple text formatting, or complex macros for automation.

See [Install PyPN]({{ site.url }}/install_pypn/) for installation instructions.

After PyPN is installed, go to View > Window > Scripts (or press Alt+F10).  You should be able to see the Scripts menu, with the sample scripts listed.

Double click on a line in the Scripts window to run that script.

To edit the scripts, go to "\Program Files\Programmer's Notepad\Scripts" and look at `text.py`.  There you'll see the source code for the sample scripts, and you can add your own.  You may have to restart PN to show changes.

## PyPN Scripting

The PyPN extension makes it pretty easy to add your own features to Programmer's Notepad. PyPN exposes the underlying Scintilla interface to Python and this results in a very powerful scripting ability. The PyPN programmer has the PN objects, Scintilla editor, and the entire installed Python library ready for import. 

## Getting Started

To begin, start by creating a new document and selecting the "Python" scheme. Once the scheme is set to Python type `pnscriptfile` into the document, then press `Ctrl+Alt+Space`. This should auto-fill the document with this basic code template:

```python
###############################################################################
## Script description
## By: your name here

import pn
import scintilla
from pypn.decorators import script

@script()
def ():
    """ What this does """
    pass
```

The top part of the file is a header area where you can fill in some information about your script, which may help should you decide to share your script or refer back to it at some point in the future.

The next few lines are the standard imports needed for most PyPN scripts. These will give you access to the pn library, the scintilla library, and will expose the `@script()` decorator that you can see on the next line down.

The script decorator allows PN to parse out some meta-data from the script file that lets your script appear in the list of scripts on the Scripts window (`Alt+F10`). Generally this would have the syntax: `@script("Script Name", "Group")`

However if this is left blank it will default to the name of the method for "Script Name" and will put the script in the "Python" group.

The next line is where you define you method. In this case lets create the method `HelloPN`.

```python
###############################################################################
## Hello Programmer's Notepad
## By: PyPN Programmer

import pn
import scintilla
from pypn.decorators import script

@script()
def HelloPN():
    """ Say Hello To Programmer's Notepad """
    newDoc = pn.NewDocument(None)
    editor = scintilla.Scintilla(newDoc)
    message = "Hello Programmer's Notepad!"
    editor.BeginUndoAction()
    editor.AppendText(len(message), message)
    editor.EndUndoAction()
    pass
```

Save this file as `HelloPN.py` inside of the `<Programmers Notepad Home Dir>\scripts` directory. Once saved, exit Programmer's Notepad and restart. When you bring up the scripts window you should see a new script under the `Python` group. Double clicking on this script will run it. If you do not see the script, then look in the output window for a Python error which may help you in determining what went wrong.

Taking a look at the method just defined:

```
python""" Say Hello To Programmer's Notepad """
```

The three quotes on an empty line begins a documentation comment. So this line aids in documenting what the script does.

```
pythonnewDoc = pn.NewDocument(None)
```

This line instructs PN to create a new document with no schema. Had we used `pn.NewDocument("Python")` we would have created a new Python document. Had we used `pn.CurrentDoc()` we not have created a new document but found a reference to the current document -- allowing us to append our message to the end of whatever document was currently selected. 

```
pythoneditor = scintilla.Scintilla(newDoc)
```

To edit documents we need access to the Scintilla interface. This line creates a Scintilla editor object for the document we just created. We need an editor object to access the text of a document.

```
pythonmessage = "Hello Programmer's Notepad!" 
```

This created a string with our message text in it. We will use this string to insert our message into the document.

```
pythoneditor.BeginUndoAction()
```

It is never any fun when an editor preforms an action that cannot be undone. This line will establish an "undo point" so that the user can undo the operation that our script did to the document. The user will be able to undo any operations preformed between this point and when the `EndUndoAction()` is called. In this case, pressing Ctrl-Z on the new document should remove our message.

```
pythoneditor.AppendText(len(message), message)
```

This line will append our message to the end of the document. The `AppendText(len, text)` function takes two arguments. The first parameter is the number of characters you would like it to take from the `text` string provided in the second parameter.

```
pythoneditor.EndUndoAction()
```

This line will mark and end of undo operations. It is advisable to ensure that you always make changes to documents inside of an undo block so that the user can revert any changes should they be disappointed in the results.

```
pass
```

This line has no effect. The `pass` statement in Python has no operation and is merely used as a place holder or marker. In this case it serves as a marker for the end of the current function. It can be removed with no effect upon the functionality or logic of the code.

## User examples

For a list of user generated scripts please see the [List of User-Submitted PyPN Scripts]({{ site.url }}/list_of_user-submitted_pypn_scripts/).

## PyPn API

The [PyPN API]({{ site.url }}/pypn_api_pages/) pages.

