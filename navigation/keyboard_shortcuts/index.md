---
layout: up
title: "Keyboard Shortcuts"
---
# Keyboard Shortcuts

This help describes some of the default keyboard shortcuts used by PN2. You can find and customise all shortcuts in the options dialog.

## The Escape Key

The escape key can often be used to get you out of dialogs - it generally represents the pressing of the cancel button in these cases. The escape button can also be used to hide any output, find in files or find bar windows that are visible. So if you run a compile which shows you some output, and then you don't want to see the output window any more then just press escape until it goes away!

## Window Navigation

To go to the next window, you can use either Ctrl-Tab or Ctrl-F6. Depending on your options, this will either navigate through documents in Windows' own recent window order, or will use a Visual-Studio window stack system. Note that you can hold down the Shift key with either of these combinations to traverse in the opposite direction.

If you edit many files at the same time, their tabs may not fit the window width. In such a case, you can use the two little arrows (next to the close document button) to scroll through them.

## Output Window

There are two types of output window (from 0.4 onwards). The global output window (dockable) can be toggled with F8, and individual output windows can be toggled with Shift-F8.

## Indent Unindent

Select a block of text over a line and use the Tab key to indent (by either tab characters or spaces depending on your settings). Use Shift-Tab to unindent.

## More Shortcuts

Here are some default shortcuts sorted on the left by shortcut and the right by command name.

| Shortcut  |  Command  |  Shortcut  |  Command  |
| --- | --- | --- | --- |
| F2 | Next Bookmark | Ctrl+Space | Autocomplete  |
| F3 | Find Next  | Ctrl+Shift+C | Clipboard Swap |
| F3 | Find Next | Ctrl+F4 | Close Window |
| F8 | Toggle Output Window | Ctrl+W | Close Window |
| Alt+F6 | Toggle Project Window | Ctrl+Insert | Copy |
| Alt+F7 | Toggle Text Clips Window | Ctrl+C | Copy |
| Alt+F8 | Toggle Find Results Window | Ctrl+Alt+C | Copy as RTF |
| Alt+F9 | Toggle CTags Window | Ctrl+Shift+T | Copy Line |
| Alt+F10 | Toggle Scripts Window | Shift+Delete | Cut |
| Alt+Enter | Show Document Properties | Ctrl+X | Cut |
| Alt+G | Jump To (tags) | Ctrl+L | Cut Line |
| Ctrl+F2 | Set Bookmark | Ctrl+Shift+L | Delete Line |
| Ctrl+F4 | Close Window | Ctrl+D | Duplicate Line |
| Ctrl+F6 | Next Window | Ctrl+F | Find Dialog |
| Ctrl+Space | Autocomplete  | Ctrl+Shift+F | Find in Files Dialog |
| Ctrl+Tab | Next Window | F3 | Find Next |
| Ctrl+Insert | Copy | F3 | Find Next  |
| Ctrl+Num * | Quick Find  | Shift+F3 | Find Previous  |
| Ctrl+/ | Quick Find  | Ctrl+G | Goto |
| Ctrl+A | Select All | Shift+Insert | Insert |
| Ctrl+C | Copy | Alt+G | Jump To (tags) |
| Ctrl+D | Duplicate Line | Ctrl+U | Lowercase |
| Ctrl+F | Find Dialog | Ctrl+N | New File |
| Ctrl+G | Goto | F2 | Next Bookmark |
| Ctrl+H | Replace Dialog | Ctrl+F6 | Next Window |
| Ctrl+L | Cut Line | Ctrl+Tab | Next Window |
| Ctrl+N | New File | Ctrl+O | Open File |
| Ctrl+O | Open File | Ctrl+V | Paste |
| Ctrl+P | Print | Ctrl+Shift+F6 | Previous Window |
| Ctrl+R | Replace Dialog | Ctrl+Shift+Tab | Previous Window |
| Ctrl+S | Save | Ctrl+P | Print |
| Ctrl+T | Transpose Lines | Ctrl+Num * | Quick Find  |
| Ctrl+U | Lowercase | Ctrl+/ | Quick Find  |
| Ctrl+V | Paste | Ctrl+Y | Redo |
| Ctrl+W | Close Window | Ctrl+H | Replace Dialog |
| Ctrl+X | Cut | Ctrl+R | Replace Dialog |
| Ctrl+Y | Redo | Ctrl+S | Save |
| Ctrl+Z | Undo | Ctrl+Shift+S | Save All |
| Shift+F3 | Find Previous  | Ctrl+A | Select All |
| Shift+F8 | Toggle Individual Output Window | Ctrl+F2 | Set Bookmark |
| Shift+Delete | Cut | Alt+Enter | Show Document Properties |
| Shift+Insert | Insert | Ctrl+Shift+H | Switch to (or Open) Alternate file |
| Ctrl+Alt+C | Copy as RTF | Alt+F9 | Toggle CTags Window |
| Ctrl+Shift+F6 | Previous Window | Alt+F8 | Toggle Find Results Window |
| Ctrl+Shift+Tab | Previous Window | Shift+F8 | Toggle Individual Output Window |
| Ctrl+Shift+C | Clipboard Swap | F8 | Toggle Output Window |
| Ctrl+Shift+F | Find in Files Dialog | Alt+F6 | Toggle Project Window |
| Ctrl+Shift+H | Switch to (or Open) Alternate file | Alt+F10 | Toggle Scripts Window |
| Ctrl+Shift+L | Delete Line | Alt+F7 | Toggle Text Clips Window |
| Ctrl+Shift+S | Save All | Ctrl+T | Transpose Lines |
| Ctrl+Shift+T | Copy Line | Ctrl+Z | Undo |
| Ctrl+Shift+U | Uppercase | Ctrl+Shift+U | Uppercase |

Additionally once can select blocks of text (rectangular selections) using Alt+Shift and then moving the cursor either with with the mouse or by keyboard navigation.

## ASCII Control Characters
Most ASCII control characters can be inserted with Ctrl+Shift plus the corresponding letter.  

Examples:

| Control character | Caret notation | Key Sequence |
| --- | --- | --- |
| NUL | `^@` | Ctrl-Shift-2 |
| EOT | `^D` | Ctrl-Shift-D |
| BEL | `^G` | Ctrl-Shift-G |
| TAB | `^I` | Ctrl-Shift-I |
| CR  | `^M` | Ctrl-Shift-M |

Wikipedia has a [list of ASCII control characters](http://en.wikipedia.org/wiki/ASCII#ASCII_control_characters) and the corresponding caret notations.


## Selection
Linear select: Shift+Cursor Movement such as shift+page down, or shift+ctrl+right arrow (select till next word boundary).

Box Select: Alt+selection. Again Alt+dragging mouse or using alt+shift+arrow keys or other navigation keys (home, page up, page down etc.) will select a block.

Multi-Select: ctrl + selection. Multi-select uses the multiple-edit feature of placing the cursor in multiple locations. For this reason it is best to use the mouse for selection. Simply make a selection. Then press ctrl and use the mouse to choose a new cursor location by pressing the left mouse button. Drag the cursor with the mouse to select the new text.

Note that with multi-select paste will only work in the last selection made but will clear all other selections. The same is true with placing the cursor in multiple places -- paste will only occur at the last one (but typing will occur at all cursor locations).


