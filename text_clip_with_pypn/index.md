---
layout: up
title: Text Clips with PyPN
---

# Text Clips with PyPN

As of PN 2.1.5/PyPN 0.12 you can insert python scripts into Text Clips greatly expanding their functionality.

You can insert python scripts in between backquotes/backticks (``).

```
  `print "hello world"`
```

inserts the following text: hello world

All stdout output is captured and inserted into the text clip.

## Examples

**Placing highlighted text into an HTML paragraph tag:**

Text Clip:

```
<p>${PN_SELECTEDTEXT}</p>`scintilla.Scintilla(pn.CurrentDoc()).Clear()`
```

This uses the [Variable](text_clip_variables/) ''PN_SELECTEDTEXT'' to insert these currently selected text into the current Text Clip, then it uses PyPN's `Scintilla::Clear()` function to clear the currently selected text. 

**Replicate the Current Line:**

Text Clip:

```
  `scintilla.Scintilla(pn.CurrentDoc()).LineDuplicate()`
```

For this you can set the short cut to something like 'rep' and then at the end of a line type 'rep' and press the autocomplete (ctrl+alt+space by default) and the current line will be repeated.