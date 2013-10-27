---
layout: up
title: PyPN API Pages
---
# PyPN API Description

The API for Programmer's Notepad is based mostly upon three modules: **pn**, **scintilla**, and **glue**. The `pn` module serves as a base and provides some basic application functionality as well as access to the other APIs. The `scintilla` module provides access to the Scintilla editor. Finally, the `glue` module provides a way for scripts to bind themselves into the Programmer's Notepad event model. 

## The pn Module

Utilities for working with Programmer's Notepad, entry point for all APIs. `import pn`

### pn Data Elements

| pn : Data | |
| --- | --- |
| `pn.IDOK : 1` | User's response was OK. |
| `pn.IDCANCEL : 2` | User's response was Cancel. |
| `pn.IDYES : 6` | User's response was Yes. |
| `pn.IDNO : 7` | User's response was No. |
| `pn.MB_ICONERROR : 16` | Display an error icon. |
| `pn.MB_ICONINFORMATION : 64` | Display an information icon. |
| `pn.MB_ICONQUESTION : 32` | Display a question icon. |
| `pn.MB_OK : 0` | OK MessageBox |
| `pn.MB_OKCANCEL : 1` | OK/Cancel MessageBox|
| `pn.MB_YESNOCANCEL : 3` | Yes/No/Cancel MessageBox |
| `pn.MB_YESNO : 4` | Yes/No MessageBox |

### pn Methods

| pn : Methods | |
| --- | --- |
| `pn.AddOutput(text) : None` | Place text to output window. |
| `pn.AppPath() : str` | Return the path to the application. |
| `pn.ClearOutput() : None` | Clears the current contents of the output window. |
| `pn.CurrentDoc() : IDocument` | Returns the currently selected document |
| `pn.GetUserSearchOptions() : ISearchOptions` | Get the object storing the user's current search options |
| `pn.InputBox(title, prompt) : str` | Ask the user for input - result is a string.|
| `pn.MessageBox(prompt, title, type) :  int` | Presents a MessageBox to the user. Note the type is determined by the constants MB_XXX. Result is an int indicating the responce. |
| `pn.NewDocument(scheme_name) : IDocument` | Creates a new document |
| `pn.OpenDocument(fullFileName, schemeName) : IDocument` | Opens an existing document. Note the scheme name can be None but it must be entered. |
| `pn.RegisterScript(func, name, group) : None` | Registers a script with PN and updates the script window. |
| `pn.SetOutputBasePath(path) : None` | Set the path to use to make relative paths parsed in output text absolute. |
| `pn.SetOutputDefaultParser() : None` | Switch to the default output parser |
| `pn.SetOutputRegEx(regexStr) : None` | Set the regular expression used to parse output text for errors |
| `pn.StringFromPointer(ptr) : str` | Get a string value from a c-style string pointer |

### pn Classes

#### FindNextResult

| pn.FindNextResult : class | |
| --- | --- |
| Enumeration used to determine the outcome of a search.||
| **pn.FindNextResult : Data**  ||
| `fnNotFound` : 0 | No results found with current search.|
| `fnFound` : 1 | A result was returned. |
| `fnReachedStart` : 2 | Search as reached the place where it began.|
| `fnInvalidRegex` : 3 | The regex is not valid. |
| `fnInvalidSearch` : 4 | The search is not valid. |
| `values` | List of the values for each enumeration object. |

#### IDocument

| pn.IDocument : class | |
| --- | --- |
| PN Document interface. | |
| **IDocument : Data**  | |
| `CanSave` | Indicates whether the document can be saved (i.e. it has a filename) |
| `CurrentScheme` | Name of the document's current scheme. (PN 2.1.5 - this value can now be set) |
| `FileName` | Full filename of the document. |
| `Modified` | Indicates whether the document has been modified |
| `Title` | Display name of the document |
| **IDocument : Methods**  | |
| `Activate() : None` | Activate the document. |
| `Close() : None` | Closes the document. |
| `FindNext(searchOptions) : FindNextResult` | Does a search in the current document. If `fnFound` is returned then the result is selected in the current document. |
| `IsValid() : bool` | Check if this document object is still valid |
| `Replace(searchOptions) : bool` | Replace the current find match based on user search settings, see `GetUserSearchOptions`. |
| `ReplaceAll(searchOptions) : int` | Replace all based on user search settings, see `GetUserSearchOptions` |
| `Save() : bool` | Save the document. |
| `SendMessage(MESSAGE, WPARAM, LPARAM) : int` | Send a message to the scintilla window. |
| `SendMessage(MESSAGE, int, string) : int` | Sends a message to the scintilla window passing text. |

#### ISearchOptions

| pn.ISearchOptions : class | |
| --- | --- |
| PN search options interface. | |
| `FileExts` | File types. |
| `FindText` | Search field text |
| `Found` |  |
| `IncludeHidden` | |
| `LoopOK` |  |
| `MatchCase` | Case sensitive |
| `MatchWholeWord` | Require match to an entire word. |
| `Recurse` | Search recursivly though folder structure. i.e. Search subdirectories. |
| `ReplaceInSelection` |  |
| `ReplaceText` | Test to use in place of match. |
| `SearchBackwards` | Search from current position to top. |
| `SearchPath` | Path of directory to do file search in. |
| `UseRegExp` | Use regular expressions in the search text field. |
| `UseSlashes` | Use backslash expressions. |

## The glue Module

Functions for hooking into pn's event handling.<code python> import glue</code>

### glue Methods

| glue : Methods | |
| --- | --- |
| `glue.evalScript(script) : None` | Evaluate a script. (PN 2.1.5) |
| `glue.finishCapturingStdOut() : None` | PN calls this to finish stdout capture and get the output. (PN 2.1.5)|
| `glue.getSchemeConfig(name) : None` | Get a pypn scheme configuration, used to hook up indenter functions and character added handlers. |
| `glue.onCharAdded(c, doc)` | Method called when a character is added, default behaviour manages calling indenters and also calls any method registered with glue.schemes[scheme].on_char_added |
| `glue.onDocLoad(doc)` | Method called when a document is loaded into PN. |
| `glue.onDocSave(filename, doc)` | Method called when a document is about to be saved to a file. |
| `glue.onDocSaved(doc)` | Method called when a document has been saved. |
| `glue.onModifiedChanged(modified, doc)` | Method called when the modified flag is changed. |
| `glue.onWriteProtectChanged(protected, doc)` | Method called when the read only status of a document is changed. |
| `glue.registerScript(f, group, scriptName)` | This is called by the script decorator to register a script. |
| `glue.runScript(name)` | This is called by PN to run a script by name. 
| `glue.startCapturingStdOut() : None` | PN calls this to start capturing stdout. (PN 2.1.5) |
| `glue.startRecording()` | PN calls this to start recording a script, all recording actions are delegated to the Recorder class. |

| glue : Data | |
| --- | --- |
| `oldstdout` | Used to temporarily hold stdout when when capturing output. (PN 2.1.5) |
| `schemes` | Tuple of SchemeMapping instances. |
| `scripts` | Tuple of pypn scripts. To run a particular script use: `scripts[name]()` (Or use `glue.runScript(name)` which does this very thing wrapped in a try block.) |


### glue Classes

#### SchemeMapping

no known data or methods.

#### StdOutCapture

| glue.StdOutCapture : class | |
| --- | --- |
| **StdOutCapture : Methods**  | |
| `getOutput() : string` | Returns the captured output. (PN 2.1.5) |
| `write(string) : None` | Adds data to the captured output. (PN 2.1.5) |


## The scintilla Module

PyPN's interface to the Scintilla editor. `import scintilla`

Most of the scintilla class' functions map directly to a Scintilla messages so you should also check the [Scintilla Documentation](http://www.scintilla.org/ScintillaDoc.html) for a more complete description of what the functions do. If you convert the function name to all caps and prefix `SCI_` then you will have the name of the Scintilla message that the function is based upon. For Example the function `GetAnchor()` maps to [SCI_GETANCHOR](http://www.scintilla.org/ScintillaDoc.html#SCI_GETANCHOR). 

For a list of the Scintilla message numbers you can look though the [scintilla interface](http://scintilla.hg.sourceforge.net/hgweb/scintilla/scintilla/file/tip/include/Scintilla.iface). These messages can be used with the Document's SendMessage function.

## scintilla Classes

### Scintilla

| scintilla.Scintilla : class | |
| --- | --- |
| The Scintilla class is the primary interface into the document's scintilla editor. You can get an instance of the class by using its constructor passing in the the document that you would like to edit: `editor = scintilla.Scintilla(pn.CurrentDoc())` | |

#### Scintilla Methods

The Scintilla class is a library wrapper for the scintilla editor -- this means that most of the methods below correspond to an equivalent Scintilla message. You can gain additional information from the [[http://www.scintilla.org/ScintillaDoc.html|Scintilla API]].

| **Scintilla : Methods** | |
| --- | --- |
| `__init__(doc) : None ` |  |
| `AddRefDocument(int) : None ` |  |
| `AddSelection(int start, int end) ` | Creates a selection from `start` to `end`, keeps existing selections. |
| `AddStyledText(length, styledText) : None ` |  |
| `AddText(length, text) : None ` | Inserts text at the current position within the document. |
| `Allocate(size) : None ` | Allocate a document buffer large enough to store a given size. This will not shrink the document buffer smaller than its current contents requires. |
| `AppendText(length, text) : None ` | Appends text to the end of the document. |
| `AssignCmdKey(int key, int command) : None ` | Redefining the key bindings. |
| `AutoCActive() : bool ` | Returns true if there is an active auto-completion list and zero if there is not. |
| `AutoCCancel() : None ` | Cancels any auto-complete list displayed. |
| `AutoCComplete() : None ` | Auto-completion with current selection. This has the same effect as the tab key in the editor. |
| `AutoCGetCurrent() : int ` |  |
| `AutoCPosStart() : int ` |  |
| `AutoCSelect(str) : None ` |  |
| `AutoCSetFillUps(str) : None ` |  |
| `AutoCShow(int, str) : None ` |  |
| `AutoCStops(str) : None ` |  |
| `BackTab() : None ` | Same as user pressing Shift+Tab in the the document editor. Moves current pos backwards based upon tabsize.|
| `BeginUndoAction() : None ` | Mark an undo save point. Generally this comes in pairs with `EndUndoAction()` where everything done to the document between the `BeginUndoAction()` and `EndUndoAction()` can be undone. |
| `BraceBadLight(pos) : None ` |  |
| `BraceHighlight(pos, (int)maxReStyle) : None ` |  | 
| `BraceMatch(int) : int ` | Will match braces on the following brace character: '(', ')', '[', ']', '{', '}', '<', and '>'. It will search forward if the brace at pos is an opening brace, backwards if it is a closing brace. The match will only occur if the style's match OR the matching brace is beyond the current styling. |
| `CallTipActive() : bool ` | Returns true if a call tip window is currently open. |
| `CallTipCancel() : None ` | Cancels any displayed call tip.  |
| `CallTipPosStart() : int ` | returns the position at which `CallTipShow()` was called. |
| `CallTipSetBack(int) : None ` |  |
| `CallTipSetFore(int) : None ` |  |
| `CallTipSetForeHlt(int) : None ` |  |
| `CallTipSetHlt(hlStart, hlEnd) : None ` | Selects a region of text to display in a highlighted style. Unhilighted text is drawn in gray. |
| `CallTipShow(pos, tipText) : None ` | Displays a call tip box at the position given. If a call tip box is already active this has no effect. |
| `CanPaste() : bool ` | Returns true is document is not read only. |
| `CanRedo() : bool ` |  |
| `CanUndo() : bool ` |  |
| `Cancel() : None ` |  |
| `CharLeft() : None ` | Moves current pos left one character. |
| `CharLeftExtend() : None ` | Extends the current selection left one character. |
| `CharLeftRectExtend() : None ` | Extends the current rectangular/block selection left one character. |
| `CharRight() : None ` | Moves current pos right one character. |
| `CharRightExtend() : None ` | Extends the current selection left one character. |
| `CharRightRectExtend() : None ` |  |
| `ChooseCaretX() : None ` |  |
| `Clear() : None ` | Clears the current selection. If nothing is selected it clears the current character under the cursor. |
| `ClearAll() : None ` | If document is not read-only that this will clear the contents of the current document. |
| `ClearAllCmdKeys() : None ` |  |
| `ClearCmdKey(int) : None ` |  |
| `ClearDocumentStyle() : None ` | Clears the document styles to 0. Not the same as turning off syntax highlighting. see Colourize() to reapply the styles. |
| `ClearRegisteredImages() : None ` | Clears the images registered with the Auto-completion lists. |
| `ClearSelections() ` | Clears/removes all selections. |
| `Colourise(start, end) : None ` | Requests the current lexer to colourize the current document between start and end. |
| `ConvertEOLs(int eolStyle) : None ` | Convert the End of Line styles to. (CRLF: 0, CR: 1, LF: 2). |
| `Copy() : None ` | Copies the current selection to the clipboard. |
| `CopyRange(start, end) : None ` | Copies the text between start:end into the clipboard. |
| `CopyText(length, text) : None ` | Copies `length` character from the string `text` into the clipboard. |
| `CreateDocument() : int ` | Creates a new empty document. The document is not attached to any editor window. |
| `Cut() : None ` | Cuts the current selection from the document and places the selected text into the clipboard. |
| `DelLineLeft() : None ` | Deletes the characters on the current line from the current position to the end of the line. |
| `DelLineRight() : None ` | Deletes the character on the current line from the current position to the beginning of the line. |
| `DelWordLeft() : None ` | Deletes the character from the current position to the end of the word. |
| `DelWordRight() : None ` | Deletes the character from the current position to the beginning of the word. |
| `DeleteBack() : None ` | Deletes the char just behind the current position. |
| `DeleteBackNotLine() : None ` |  |
| `DocLineFromVisible(int) : int ` |  |
| `DocumentEnd() : None ` | Sets current position to the end of the document. |
| `DocumentEndExtend() : None ` | Extends the current selection to the end of the document. |
| `DocumentStart() : None ` | Sets current position to the beginning of the document. |
| `DocumentStartExtend() : None ` | Extends the current selection to the beginning of the document.  |
| `EditToggleOvertype() : None ` | Toggles overwrite mode. |
| `EmptyUndoBuffer() : None ` | Clears the undo buffer. |
| `EndUndoAction() : None ` | Mark the end of an |
| `EnsureVisible(int) : None ` |  |
| `EnsureVisibleEnforcePolicy(int) : None ` |  |
| `FindColumn(int, int) : int ` |  |
| `FindText(int, int, str, int) : object ` |  |
| `FormFeed() : None ` |  |
| `FormatRange(bool, int) : int ` |  |
| `GetCharAt(pos) : int ` | Get the character at the given position. |
| `GetCurLine() : tuple ` | Returns a tuple containing the current line and the current column [line, col]. To get the text of the current line use first value of the tuple. i.e. `scintilla.Scintilla(pn.CurrentDoc()).GetCurLine()[0]` gives the text of the current line. `scintilla.Scintilla(pn.CurrentDoc()).GetCurLine()[1]` gives the current column.|
| `GetLine(lineNum) : str ` | Get the line at `lineNum`.|
| `GetLineEndPosition(lineNum) : int ` | Return the position of the EOL for line number `lineNum` |
| `GetLineIndentPosition(int) : int ` |  |
| `GetLineIndentation(int) : int ` |  |
| `GetLineSelEndPosition(int) : int ` | Get the ending line number of the current selection. |
| `GetLineSelStartPosition(int) : int ` | Get the beginning line number of the current selection. |
| `GetLineState(int) : int ` |  |
| `GetLineVisible(int) : bool ` |  |
| `GetPropertyInt(str, int) : int ` |  |
| `GetSelectionNAnchor(int selNum) : int ` | Get anchor position (where the selection started) of Nth selection part, N=0 for the first position. |
| `GetSelectionNCaret(int selNum) : int ` | Get caret position (where the selection ended) of Nth selection part. | |
| `GetSelectionNEnd(int selNum) : int ` | Get start position of the Nth selection part, returns either anchor or caret position according to which is lesser. |
| `GetSelectionNStart(int selNum) : int ` | Get end position of the Nth selection part, returns either anchor or caret position according to which is greater. |
| `GetStyleAt(pos) : int ` | Get the style at the position given by pos. |
| `GetStyledText(object) : int ` |  |
| `GetText(int) : str ` |  |
| `GetTextRange(start, end) : str ` | Get the text between postions `start` and `end` |
| `GotoLine(lineNum) : None ` | Move the current position to the beginning of of line `lineNum` |
| `GotoPos(pos) : None ` | Move the current position to `pos` |
| `GrabFocus() : None ` | Make this document current focus. |
| `HideLines(int, int) : None ` |  |
| `HideSelection(bool) : None ` |  |
| `Home() : None ` | Goto the start of the current line (after indention) |
| `HomeDisplay() : None ` |  |
| `HomeDisplayExtend() : None ` |  |
| `HomeExtend() : None ` | Extend the selection to the start of the current line (after indention) |
| `HomeRectExtend() : None ` |  |
| `HomeWrap() : None ` |  |
| `HomeWrapExtend() : None ` |  |
| `IndentLine(int, int) : None ` |  |
| `IndicGetFore(int) : int ` |  |
| `IndicGetStyle(int) : int ` |  |
| `IndicSetFore(int, int) : None ` |  |
| `IndicSetStyle(int, int) : None ` |  |
| `InsertText(len, text) : None ` | Insert text into document at the current location. |
| `LineCopy() : None ` |  |
| `LineCut() : None ` |  |
| `LineDelete() : None ` |  |
| `LineDown() : None ` | Move the cursor up one line. |
| `LineDownExtend() : None ` | Extend the current selection down one line. |
| `LineDownRectExtend() : None ` | Extend the current rectangular/block selection down one line. |
| `LineDuplicate() : None ` | Duplicate the current line. |
| `LineEnd() : None ` | Move the cursor to the end of the current line. |
| `LineEndDisplay() : None ` | Move the cursor to the end of the currently display line. When word wrap is enabled this moves the cursor to the end of the current display line but not the end of the current line of the document. |
| `LineEndDisplayExtend() : None ` | Extend the current rectangular/block selection to the end of the current display line.  |
| `LineEndExtend() : None ` | Extend the current selection to the end of the current line.  |
| `LineEndRectExtend() : None ` | Extend the current rectangular/block selection to the end of the current line. |
| `LineEndWrap() : None ` |  |
| `LineEndWrapExtend() : None ` |  |
| `LineFromPosition(position) : int ` | returns the line number for the line that contains the character position. The return value is 0 if `position <= 0`. The return value is the last line if position is past the end of the document. |
| `LineLength(int) : int ` | This returns the length of the line, including any line end characters. If line number is invalid, the result is 0. To get the length of the line not including any end of line characters, you can use `GetLineEndPosition(line) - PositionFromLine(line)`. |
| `LineScroll(int, int) : None ` |  |
| `LineScrollDown() : None ` |  |
| `LineScrollUp() : None ` |  |
| `LineTranspose() : None ` | Swap the current line with the one directly above it (has no affect on first line). |
| `LineUp() : None ` | Move the cursor up one line. |
| `LineUpExtend() : None ` | Extend the selection up one line |
| `LineUpRectExtend() : None ` | Extend the rectangular/block selection up one line. |
| `LinesJoin() : None ` |  |
| `LinesSplit(int) : None ` |  |
| `LoadLexerLibrary(str) : None ` |  |
| `LowerCase() : None ` | Converts the current selection to lower case. |
| `MarkerAdd(line, number) : int ` | Adds a Marker to the left hand margin. The displayed marker number is 1 - number. Returns a handle to the marker or -1 if the marker was not net. |
| `MarkerDefine(markerNumber, symbol) : None ` | Defines a marker. For a description of the various symbols please see the scintilla documentation. |
| `MarkerDefinePixmap(int, str) : None ` |  |
| `MarkerDelete(line, markerNumber) : None ` | Removes a marker. If markerNumber is set to -1 the all markers on that line are removed. |
| `MarkerDeleteAll(markerNumber) : None ` | Deletes all instances of a given markerNumber. |
| `MarkerDeleteHandle(markerHandle) : None ` | Deletes a marker based upon its handle. The handle is returned by MarkerAdd. |
| `MarkerGet(line) : int ` | Returns a 32bit number defining what markers are defined on a line. Marker 0 is bit 0, Marker 1 is bit 1 etc. |
| `MarkerLineFromHandle(markerHandle) : int ` | Returns the line number for a given marker handle. This will return the line number after operations that change the line orderings. |
| `MarkerNext(startLine, mask) : int ` | Finds the next line that has a marker defined in the mask. For the mask marker 0 is bit 0, marker 1 is bit 1 etc... There are 32 markers available.|
| `MarkerPrevious(startLine, mask) : int ` | Finds the previous (searching up) line that has a marker defined in the mask. For the mask marker 0 is bit 0, marker 1 is bit 1 etc... There are 32 markers available. |
| `MarkerSetBack(markerNumber, colour) : None ` | Sets the marker's background colour. |
| `MarkerSetFore(markerNumber, colour) : None ` | Sets the marker's foreground colour. |
| `MoveCaretInsideView() : None ` | If the cursor is not in the current view (above or below what is currently displayed), it is moved to the nearest line that is visible to its current position. Any selection is lost. Note this does not affect the cursors horizontal position. |
| `NewLine() : None ` | Inserts a new lines as though the user pressed the enter key. |
| `PageDown() : None ` | Moves the current position down one page as though the user pressed the Page down key. |
| `PageDownExtend() : None ` | Extends the current selection down one page. Same as Shift+Page Down. |
| `PageDownRectExtend() : None ` | Extends the current rectangular/block selection down one page. Same as Alt+Shift+Page Down.  |
| `PageUp() : None ` | Moves the current position up one page as though the user had pressed the Page Up key. |
| `PageUpExtend() : None ` | Extends the current selection up one page as though the user had pressed shift+Page Up. |
| `PageUpRectExtend() : None ` | Extends the current rectangular/block selection up one page as though the user had pressed Alt+Shift+Page Up.  |
| `ParaDown() : None ` | Moves the current position down the beginning of the next paragraph.  |
| `ParaDownExtend() : None ` | Extends the current selection down to the beginning of the next paragraph. |
| `ParaUp() : None ` | Moves the current position up to the beginning of the next paragraph. |
| `ParaUpExtend() : None ` | Extends the current selection up to the beginning of the next paragraph. |
| `Paste() : None ` | Pastes the textual content of the clipboard into the document at the current position |
| `PointXFromPosition(pos : int) : int ` | Returns the x-coordinate for the given position. |
| `PointYFromPosition(pos : int) : int ` | Returns the y-coordinate for the given position. |
| `PositionAfter(int) : int ` |  |
| `PositionBefore(int) : int ` |  |
| `PositionFromLine(int) : int ` |  |
| `PositionFromPoint(int, int) : int ` |  |
| `PositionFromPointClose(int, int) : int ` |  |
| `Redo() : None ` |  |
| `RegisterImage(int, str) : None ` |  |
| `ReleaseDocument(int) : None ` |  |
| `ReplaceSel(str) : None ` |  |
| `ReplaceTarget(int, str) : int ` |  |
| `ReplaceTargetRE(int, str) : int ` |  |
| `ScrollCaret() : None ` |  |
| `SearchAnchor() : None ` |  |
| `SearchInTarget(int, str) : int ` |  |
| `SearchNext(int, str) : int ` |  |
| `SearchPrev(int, str) : int ` |  |
| `SelectAll() : None ` | Sets the selection start to 0 and the selection end to the length of the document. |
| `SetCharsDefault() : None ` |  |
| `SetFoldFlags(flags : int) : None ` | Sets visual markers in the document (as apposed to the margin) for folding. 1 = Experimental, Draw Boxes 2 = Draw line above if expanded 4 = Draw line above if collapsed 8 = Draw line below if expanded 16 = Draw line below if collapsed |
| `SetFoldMarginColour(bool, int) : None ` |  |
| `SetFoldMarginHiColour(bool, int) : None ` |  |
| `SetHotspotActiveBack(bool, int) : None ` |  |
| `SetHotspotActiveFore(bool, int) : None ` |  |
| `SetHotspotActiveUnderline(bool) : None ` |  |
| `SetHotspotSingleLine(bool) : None ` |  |
| `SetKeyWords(keyWordSet : int, keyWordList : str) : None ` | You can set up to 9 sets of keywords. `keyWordSet` integer from 0 to 8 indicating the keyword set you would like to set. `keyWordList` is a list of keywords separated by whitespace (\x20,\n,\r,\t). Words must be made of non-whitespace characters. How the keywords are used is entirely up to the lexer. |
| `SetLexerLanguage(str) : None ` |  |
| `SetLineIndentation(int, int) : None ` |  |
| `SetLineState(int, int) : None ` |  |
| `SetProperty(str, str) : None ` |  |
| `SetSavePoint() : None ` | Marks the document as unchanged. This would be called right after a save operation to let scintilla know the document is unmodified. **Can not undo** from this point. |
| `SetSel(start, end) : None ` | Sets the current selection. |
| `SetSelection(start, end) : None ` | Same as `SetSel()` |
| `SetSelBack(bool, int) : None ` |  |
| `SetSelFore(bool, int) : None ` |  |
| `SetStyling(int, int) : None ` |  |
| `SetStylingEx(int, str) : None ` |  |
| `SetText(str) : None ` | Sets the text of the current document. |
| `SetVisiblePolicy(int, int) : None ` |  |
| `SetWhitespaceBack(bool, int) : None ` |  |
| `SetWhitespaceChars(str) : None ` | Sets the  whitespace characters. |
| `SetWhitespaceFore(bool, int) : None ` |  |
| `SetWordChars(str) : None ` | Sets the word characters. Words are defined to be contiguous sequences of these characters. This definition is used in scintilla's movement and manipulation routines.|
| `SetXCaretPolicy(int, int) : None ` |  |
| `SetYCaretPolicy(int, int) : None ` |  |
| `ShowLines(int, int) : None ` |  |
| `StartRecord() : None ` |  |
| `StartStyling(int, int) : None ` |  |
| `StopRecord() : None ` |  |
| `StutteredPageDown() : None ` |  |
| `StutteredPageDownExtend() : None ` |  |
| `StutteredPageUp() : None ` |  |
| `StutteredPageUpExtend() : None ` |  |
| `StyleResetDefault() : None ` |  |
| `StyleSetBack(int, int) : None ` |  |
| `StyleSetBold(int, bool) : None ` |  |
| `StyleSetCase(int, int) : None ` |  |
| `StyleSetChangeable(int, bool) : None ` |  |
| `StyleSetCharacter(int, int) : None ` |  |
| `StyleSetClearAll() : None ` |  |
| `StyleSetEOLFilled(int, bool) : None ` |  |
| `StyleSetFont(int, str) : None ` |  |
| `StyleSetFore(int, int) : None ` |  |
| `StyleSetHotSpot(int, bool) : None ` |  |
| `StyleSetItalic(int, bool) : None ` |  |
| `StyleSetSize(int, int) : None ` |  |
| `StyleSetUnderline(int, bool) : None ` |  |
| `StyleSetVisible(int, bool) : None ` |  |
| `Tab() : None ` | Same as user pressing the tab key. |
| `TargetAsUTF8(str) : int ` |  |
| `TargetFromSelection() : None ` |  |
| `TextHeight(int) : int ` | This returns the height in pixels of a particular line. Currently all lines are the same height. |
| `TextWidth(styleNumber, str) : int ` | This returns the pixel width of a string drawn in the given styleNumber which can be used, for example, to decide how wide to make the line number margin in order to display a given number of numerals. |
| `ToggleCaretSticky() : None ` |  |
| `ToggleFold(int) : None ` |  |
| `Undo() : None ` | Undo. |
| `UpperCase() : None ` | Converts the current selection to upper case. |
| `UsePopUp(bool) : None ` |  |
| `UserListShow(int, str) : None ` |  |
| `VCHome() : None ` | Moves cursor to beginning of current line. |
| `VCHomeExtend() : None ` | Extends the current selection to the beginning of the current line. |
| `VCHomeRectExtend() : None ` | Extends the current rectangular/block selection to the beginning of the current line. |
| `VCHomeWrap() : None ` |  |
| `VCHomeWrapExtend() : None ` |  |
| `VisibleFromDocLine(lineNumber) : int ` | Returns the beginning display line given a document line. In word wrap a given document line may occupy several display lines, if word wrap is not used this should return the same value as document line. |
| `WordEndPosition(int, bool) : int ` | Moves the current position to the end of the current word. |
| `WordLeft() : None ` | Moves the current position to the beginning of the next word to the left. |
| `WordLeftEnd() : None ` | Moves the current position to the end of the next word to the left. |
| `WordLeftEndExtend() : None ` | Extends the current selection to the end of the next word to the left. |
| `WordLeftExtend() : None ` | Extends the current selection to the beginning of the next word to the left. |
| `WordPartLeft() : None ` |  |
| `WordPartLeftExtend() : None ` |  |
| `WordPartRight() : None ` |  |
| `WordPartRightExtend() : None ` |  |
| `WordRight() : None ` | Moves the current position to the beginning of the next word to the right.  |
| `WordRightEnd() : None ` | Moves the current position to the end of the next word to the right. |
| `WordRightEndExtend() : None ` | Extends the current selection to the end of the next word to the right. |
| `WordRightExtend() : None ` | Extends the current selection to the beginning of the next word to the right. |
| `WordStartPosition(int, bool) : int ` | Find the position of the first char of the current word. |
| `ZoomIn() : None ` | Uniformly increases the size of the text in this document. |
| `ZoomOut() : None ` | Uniformly decreases the size of the text in this document. |

#### Scintilla Data

| **Scintilla : Data** | |
| --- | --- |
| `Anchor` | Selections are always between the anchor and the current location. Note that this is not necessarily the same as the selection start. |
| `AutoCAutoHide` |  |
| `AutoCCancelAtStart` |  |
| `AutoCChooseSingle` |  |
| `AutoCDropRestOfWord` |  |
| `AutoCIgnoreCase` |  |
| `AutoCSeparator` |  |
| `AutoCTypeSeparator` |  |
| `BackSpaceUnIndents` |  |
| `BufferedDraw` |  |
| `CaretFore` |  |
| `CaretLineBack` |  |
| `CaretLineVisible` |  |
| `CaretPeriod` |  |
| `CaretSticky` |  |
| `CaretWidth` |  |
| `CodePage` |  |
| `Column` |  |
| `ControlCharSymbol` |  |
| `CurrentPos` | The current position of the cursor. |
| `Cursor` |  |
| `DirectFunction` |  |
| `DirectPointer` |  |
| `DocPointer` |  |
| `EOLMode` |  |
| `EdgeColour` |  |
| `EdgeColumn` |  |
| `EdgeMode` |  |
| `EndAtLastLine` |  |
| `EndStyled` |  |
| `FirstVisibleLine` |  |
| `Focus` |  |
| `FoldExpanded` |  |
| `FoldLevel` |  |
| `FoldParent` |  |
| `HScrollBar` |  |
| `HighlightGuide` |  |
| `Indent` |  |
| `IndentationGuides` |  |
| `LastChild` |  |
| `LayoutCache` |  |
| `Length` |  |
| `Lexer` |  |
| `LineCount` |  |
| `LinesOnScreen` |  |
| `MarginLeft` |  |
| `MarginMaskN` |  |
| `MarginRight` |  |
| `MarginSensitiveN` |  |
| `MarginTypeN` |  |
| `MarginWidthN` |  |
| `MaxLineState` |  |
| `ModEventMask` |  |
| `Modify` |  |
| `MouseDownCaptures` |  |
| `MouseDwellTime` |  |
| `Overtype` |  |
| `PrintColourMode` |  |
| `PrintMagnification` |  |
| `PrintWrapMode` |  |
| `ReadOnly` |  |
| `RectangularSelectionAnchor` | Gets the rectangular/block selection anchor position. |
| `RectangularSelectionCaret` | Gets the rectangular/block selection cursor position. |
| `ScrollWidth` |  |
| `SearchFlags` |  |
| `SelText` | The currently selected text.  |
| `SelectionEnd` |  |
| `SelectionIsRectangle` | Whether there's currently a rectangular/block selection or not. |
| `SelectionMode` |  |
| `SelectionStart` |  |
| `Status` |  |
| `StyleBits` |  |
| `TabIndents` |  |
| `TabWidth : int` | The number of spaces equivalent to a tab. |
| `TargetEnd` |  |
| `TargetStart` |  |
| `TextLength` |  |
| `TwoPhaseDraw` |  |
| `UndoCollection` |  |
| `UsePalette` |  |
| `UseTabs` |  |
| `VScrollBar` |  |
| `ViewEOL` |  |
| `ViewWS` |  |
| `WrapMode` |  |
| `XOffset` |  |
| `Zoom : int` | The current zoom level. |
