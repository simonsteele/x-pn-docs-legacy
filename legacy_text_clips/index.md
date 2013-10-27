---
layout: up
title: Legacy Text Clips
---

**This information applies to releases prior to 2.1.4**

# What are Text Clips?

Text clips are text fragments that are easily inserted into your code. Programmer's Notepad ships with a variety of text clips representing common programming languages such as C or ASP.NET, or common character sets such as the symbols associated with HTML.

There are two types of Text Clips you can use:

  - Code Templates: These clips can be inserted from Edit|Insert Template or by using the keyboard shortcut.
  - Browsable Clips: These are inserted by selecting a clip from the Text Clips window.

## Code Templates

Code Templates can be edited in the Options dialog, and are referenced by a shortcut. For example:

```
shortcut: ife
inserts:

if
{

}
else
{

}
```

They are best used for code you want to insert regularly while typing and where a shortcut will save time. You can see a short video of templates in use here:

http://video.google.com/videoplay?docid=-8777247135013651124&hl=en

## Browsable Clips

Classic Text Clips are stored as .clips files, and cannot currently be edited from within the Programmer's Notepad user interface (yet). To install extra text clips drop the .clips files in your PN user settings directory. The files are standard XML documents and may be manipulated using any text or XML editor.

There is an external tool available for editing text clips:

[Text Clip Creator](http://www.pnotepad.org/files/textclipcreator.zip)

## Clip Format

All the text that you place in your clip text will be inserted into your document, with the exception of:

  | - the pipe character. This character is removed and the cursor placed at this position after clip insertion. To avoid this, use two pipes: ||
