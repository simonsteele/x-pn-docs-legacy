---
layout: up
title: List of User-submitted PyPN Scripts
---
# UPDATE: NEW SCRIPT SHARING SITE

**Please consider using this website to upload your scripts:**

http://scriptshare.rocketmonkeys.com/

It's a bit more organized and allows for easier browsing/uploading/viewing/downloading.  If you have any comments/suggestions on that site, please use the feedback link.  Eventually the scriptshare website's functionality will be incorporated into pnotepad.org, but for now it exists on a separate domain.

If you've already uploaded scripts to this page, consider registering on the scriptshare site and re-uploading your script there.  That will allow for things like comments, sorting, etc.

**This wiki page is now obsolete**

# List of User-Submitted PyPN Scripts

This page served as a temporary home for user-submitted scripts.  If you are adding a new script, please consider using the http://scriptshare.rocketmonkeys.com/ site instead.  This page is now obsolete.
  * Please do not modify someone else's script.  If you modify someone else's script, repost it under your name or send your changes to the original author so they can update it.  Otherwise it's chaos... chaos!

## Adding a Script

Please follow this template:
  ### [author's username] - [script title]
  [description]
  ```
  [script contents]
  ```

## Scripts

### Jumpfroggy - Sort Lines & Remove Duplicates

This is modified from the SortLines() script by Scott (wischeese).  It sorts lines and removes any duplicates.  Case sensitive.

```python
@script("Sort Lines (No Duplicates)", "Text")
def SortLinesNoDuplicates():
	""" Sort Lines, Remove Duplicates (Modified by jumpfroggy from wischeese's "SortLines" script) """
	editor = scintilla.Scintilla(pn.CurrentDoc())
	editor.BeginUndoAction()
	lsSelection = editor.GetTextRange(editor.SelectionStart, editor.SelectionEnd)
	laLines = lsSelection.splitlines(0)
	laLines.sort()
	
	# Filter out duplicate lines
	laLines2 = []
	for line in laLines:
		if line not in laLines2:
			laLines2.append(line)
	
	lsReplace = string.join(laLines2, '\r\n' )
	editor.ReplaceSel(lsReplace)
	editor.EndUndoAction() 
```

### NickDMax - ClipStack
ClipStack is a Programmer's Notepad clipboard stack. It allows the user to select text and copy the text, which gets pushed onto a stack. Later this text can be pasted, which then pops the text off of this stack. 

So, for example,  if the user copies item A, then item B, and then item C, all using ClipStack; then they could then paste these items back in the order: Item C, Item B, Item A.

ClipStack does copy the the text into the clipboard so that you can paste the current selection in another application, but this will not pop an item off of the stack.

When pasting using ClipStack the current contents of the clipboard are replaced with the item on the top of the stack. This means that anything currently in the clipboard will be lost.

What ClipStack does **not** do is push the current clipboard onto the stack. 

ClipStack could be improved by the addition of a clipboard aware python module that would allow the current clipboard to be used as the top item of the stack. This would allow the snippet to better interact with other applications.

```python
###############################################################################
## ClipStack.py -- A clipboard stack allowing you to copy items into a stack
## and then paste them back in a FILO way. So the first item copied to the stack
## is the last item pasted from it.
##  Copy item #1, Copy item #2, Copy item #3
##  Paste #3, #2, #1
##
## The last item copied is placed onto the regular OS clipboard
## The last item pasted is ALSO placed into the regular OS clipboard
##    So take care not to confuse this with the regular OS clipboard and
##    The standard Copy/Paste operations.
## By: NickDMax

import pn
import scintilla
from pypn.decorators import script

class PNClipStack:
    """ Maintains a stack of text to paste  """
    def __init__(self):
        """ initialize inner data:
            stack -- the internal list used to store clipboard items """
        self.stack = []

    def push(self, text):
        self.stack.append(text)

    def pop(self):
        retValue = ''
        if len(self.stack) > 0:
            retValue = self.stack.pop()
        return retValue

    def clear(self):
        self.stack = []

    def getSize(self):
        return len(self.stack)

ClipStack = PNClipStack()

@script('Stack Count', 'ClipStack')
def clstkCount():
    """ Prints out the size of the current ClipStack """
    pn.AddOutput('ClipStack Size: ' + str(ClipStack.getSize()) + '\n')

@script('Copy', 'ClipStack')
def clstkCopy():
    """ Adds the current selection to the ClipStack """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash our script
        editor = scintilla.Scintilla(doc)
        start = editor.SelectionStart
        end = editor.SelectionEnd
        if (start == end):  #nothing is selected lets try to grab the current word.
            start = editor.WordStartPosition(start, True)
            end = editor.WordEndPosition(end, True)
        text = None
        if (start != end):
            text = editor.GetTextRange(start, end)
        if text is not None:
            ClipStack.push(text)
            editor.CopyText(len(text), text)
            #clstkCount()

@script('Cut', 'ClipStack')
def clstkCut():
    """ Adds the current selection to the ClipStack -- cuts it from document"""
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash our script
        editor = scintilla.Scintilla(doc)
        start = editor.SelectionStart
        end = editor.SelectionEnd
        if (start == end):  #nothing is selected lets try to grab the current word.
            start = editor.WordStartPosition(start, True)
            end = editor.WordEndPosition(end, True)
        text = None
        if (start != end):
            text = editor.GetTextRange(start, end)
        if text is not None:
            ClipStack.push(text)
            editor.SetSel(start, end)
            editor.Cut()
            #clstkCount()


@script('Paste', 'ClipStack')
def clstkPaste():
    """ Pastes the top item from the ClipStack into the document """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash our script
        editor = scintilla.Scintilla(doc)
        text = ClipStack.pop()
        editor.CopyText(len(text), text)
        editor.Paste()
        #clstkCount()

@script('Clear', 'ClipStack')
def clstkCear():
    """ Clears the current ClipStack """
    ClipStack.clear()
    #clstkCount()
```

### NickDMax - Base64Utils
Use this script to encode/decode selections using base64. This can be very useful for embedding Base64 images into HTML. Simply open a small gif/png in PN, select all, and then Base64Encode. This creates a Base64 Version of your image in a new document. Prefix the encoding with ''data:image/gif;base64,'' and you have a Base64 version of your image that you can paste directly into and HTML IMG tag's source. 

```python
###############################################################################
## base64Utils.py -- PnPy utility script to encode and decode base64. 
## tested on Python 2.6.1. This utility will take the current selection
## (or document if there is no selection) and create a base 64 encoded document
## It will also take a base 64 encoded document and return the unencoded text.
## -- Note: No verification is done to ensure that the unencoded data is 
## ASCII or valid unicode textual data. 
## By: NickDMax

import pn
import scintilla
from pypn.decorators import script
import base64

@script("Base64Encode", "DocUtils")
def doBase64():
    """ This method will grab the curent selection/document and
        create a new document that is a base64 vesion of the text """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash pn too often...
        editor = scintilla.Scintilla(doc)
        start = editor.SelectionStart
        end = editor.SelectionEnd
        if (start == end):  #nothing is selected so we will just grab it all...
            start = 0
            end = editor.Length 
        text = editor.GetTextRange(start, end)
        newDoc = pn.NewDocument(None)
        newEditor = scintilla.Scintilla(newDoc)
        newEditor.BeginUndoAction()
        encoded = base64.b64encode(text)
        l = len (encoded)
        m = 0
        while l > 80:
            str = encoded[m:m+80] + '\n'
            newEditor.AppendText(len(str), str)
            l, m = l - 80, m + 80
        str = encoded[m:m+l]
        newEditor.AppendText(len(str), str)        
        newEditor.EndUndoAction()
    
@script("DecodeBase64", "DocUtils")
def undoBase64():
    """ This method will grab the curent selection/document and
        create a new document that is the base64 decoded vesion 
        of the text """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash pn too often...
        editor = scintilla.Scintilla(doc)
        start = editor.SelectionStart
        end = editor.SelectionEnd
        if (start == end):  #nothing is selected so we will just grab it all...
            start = 0
            end = editor.Length 
        text = editor.GetTextRange(start, end)
        decoded = base64.b64decode(text)
        newDoc = pn.NewDocument(None)
        newEditor = scintilla.Scintilla(newDoc)
        newEditor.BeginUndoAction()
        newEditor.AppendText(len(decoded), decoded)
        newEditor.EndUndoAction()
```

### NickDMax - "As New" Utils
Functions to extract the current selection as a new document, or paste the contents of the clipboard as a new document.

```python
###############################################################################
## AsNewUtils scripts -- the purpose of this script is to provide functions for
## extracting /pasting text as new documents.
## By: NickDMax

import pn
import scintilla
from pypn.decorators import script

@script("Extract As New", "DocUtils")
def dupDocument():
    """ This script will extract the current selection to a new document. """
    """ if there is no selection then it will duplicate the entire document. """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash pn too often...
        editor = scintilla.Scintilla(doc)
        start = editor.SelectionStart
        end = editor.SelectionEnd
        sch = doc.CurrentScheme
        if (start == end):  #nothing is selected so we will just grab it all...
            start = 0
            end = editor.Length 
        text = editor.GetTextRange(start, end)
        newDoc = pn.NewDocument(sch)
        newEditor = scintilla.Scintilla(newDoc)
        newEditor.BeginUndoAction()
        newEditor.AppendText(len(text), text)
        newEditor.EndUndoAction()
    
@script("Paste As New", "DocUtils")
def pasteAsNew():
    """ This script will paste the current contense of the clipboard into a new document """
    newDoc = pn.NewDocument(None)
    newEditor = scintilla.Scintilla(newDoc)
    newEditor.BeginUndoAction()
    newEditor.Paste()
    newEditor.EndUndoAction()
```

### NickDMax - Hex Dump Utility
Extracts the current selection as a hex dump in a new document.

```python
###############################################################################
## Doc2Hex v0.1 -- Will extract the current selection into a new document
##    Formatting the text as a Hex Dump. 
## By: NickDMax

import pn
import scintilla
from pypn.decorators import script

def HexEncode(text):
    output = ""
    if text is not None:
        lineLength = 0
        position = 0
        while len(text) > 0:
            output += "%08X |" % position
            if len(text) <= 16:
                lineLength = len(text)
            else:
                lineLength = 16
            snippet = text[0: lineLength]
            for x in snippet:
                output += " %02X" % ord(x)
            output += "\n"
            position += 16;
            text = text[lineLength:]
    return output

@script("Doc2Hex", "DocUtils")
def Doc2Hex():
    """ Doc2Hex will convert the current document into a HexDump """
    doc = pn.CurrentDoc()
    if doc is not None:     #Lets try not to crash pn too often...
        editor = scintilla.Scintilla(pn.CurrentDoc())
        start = editor.SelectionStart
        end = editor.SelectionEnd
        if (start == end):  #nothing is selected so we will just grab it all...
            start = 0
            end = editor.Length 
        text = editor.GetTextRange(start, end)
        newDoc = pn.NewDocument(None)
        newEditor = scintilla.Scintilla(newDoc)
        newEditor.BeginUndoAction()
        encoded = HexEncode(text)
        newEditor.AppendText(len(encoded), encoded)        
        newEditor.EndUndoAction()
```

### Simon Steele - Number Converter

This script converts the number you have selected into Decimal, Hexadecimal, Octal and Binary and shows the results in the output window.

```python
import pn, scintilla 

def hex2dec(s):
    """return the integer value of a hexadecimal string s"""
    return int(s, 16) 

def Denary2Binary(n):
    """convert denary integer n to binary string bStr"""
    bStr = ''
    if n < 0:
        raise ValueError, "must be a positive integer"
    if n == 0:
        return '0'
    while n > 0:
        bStr = str(n % 2) + bStr
        n = n >> 1
    return bStr 

@script("ConvertNumber")
def ConvertNumber():
    s = scintilla.Scintilla(pn.CurrentDoc())
    if s.SelectionEnd - s.SelectionStart < 1:
        return sel = s.SelText
    if sel.find('0x') != -1:
        sel = sel.replace("0x", "")
        sel = hex2dec(sel)
    else:
        sel = int(sel)
    pn.AddOutput("Dec: %d\n" % sel)
    pn.AddOutput("Hex: 0x%X\n" % sel)
    pn.AddOutput("Oct: %o\n" % sel)
    pn.AddOutput("Bin: %s" % Denary2Binary(sel))
```

### Allen Dang - Xml Beautifier
This script beautify the xml content in current active tab.
```python
import pn
import scintilla
import re, string
from pypn.decorators import script

@script("Beautify", "Xml")
def Beautify():
    editor = scintilla.Scintilla(pn.CurrentDoc())
    data = editor.GetText(editor.Length)

    fields = re.split('(<.*?>)',data)
    content = ''
    level = 0
    for f in fields:
        if string.strip(f)=='': continue
        if f[0]=='<' and f[1] != '/' and f[-2] != '/':
            content += ' '*(level*4) + f + '\n'
            level = level + 1
        elif f[0] == '<' and f[1] != '/' and f[-2] == '/':
            content += ' '*(level*4) + f + '\n'
        elif f[:2]=='</':
            level = level - 1
            content += ' '*(level*4) + f + '\n'
        else:
            content += ' '*(level*4) + f + '\n'
            
    editor.BeginUndoAction()
    editor.ClearAll()
    editor.AddText(len(content), content)
    editor.EndUndoAction()
```