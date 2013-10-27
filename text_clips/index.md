---
layout: up
title: "Text Clips"
---

# Text Clips

Text Clips are text fragments that are easily inserted into your code. Programmer's Notepad comes with a variety of Clips to help you when working with common programming languages.

Clips can be inserted either from the Text Clips window (View | Windows | Text Clips on the menu, or Alt-F7) or, for clips from the current scheme, by typing the shortcut for a clip into the editor and pressing Tab. 

![Text Clips]({{ site.url }}/assets/newclips.png)

The Text Clips window can also be used to add, edit and remove Clips, and you can group Clips to make them easier to find. Use the buttons at the bottom of the Text Clips window to do this.

To create a new clip:
  - Select the Scheme you wish to add a Clip for in the Text Clips window
  - Click on the Plus sign at the bottom of the text clips window
  - In the Text Clip Editor window that appears, type a user-friendly name, a shortcut, and the text for the clip. 
  - Press OK
  - Now you've added the clip, the shortcut you specified should be instantly available in all files with the related Scheme.

If you're not sure of the shortcut for a clip you're trying to insert, press Ctrl-Alt-Space or select Insert Text Clip from the Edit menu and you will see a completion list of currently active Clips from the current scheme.

When editing, PN will only complete Clips that are mapped to the Scheme for the file you are currently editing. To use clips from another Scheme/language use the Text Clips window to select the clip you're looking for.

**Note:** As this is a new feature, some sample clips have not had shortcuts added yet, you can edit these clips yourself to add shortcuts using the Edit Clip button in the Text Clips window (looks like a pencil).

## Clip Format

Text Clips can contain more than simple text, they can include placeholders and variables which help to insert the text you want quickly.

You can see a short video of early text clips functionality in use here:

http://video.google.com/videoplay?docid=-8777247135013651124&hl=en

At this time it was necessary to use a user-configured keyboard shortcut to insert all clips, you can now simply press tab after typing the clip shortcut to insert the matching clip.

# Placeholders

Placeholders allow you to jump through interesting points in the clip after inserting it, and also allow you to copy text into multiple places in the Clip.

For example, the following clip will let you type text into two points before leaving the cursor at the end:

    if (${1})
    {
        ${2}
    }${0}

When you first insert this clip, the cursor will be placed on placeholder 1. You can fill text in here, and then press Tab to move to placeholder 2. A final Tab will move the cursor to special placeholder 0. You can specify default text for your placeholders, allowing easy insertion of common text while allowing you to change the value at clip insertion time. For example, the following C# clip defaults to generating a public method with no return value and an empty parameter list:

    ${1:public} ${2:MethodName}(${3})
    {
        $0
    }

If you repeat a placeholder number, then the text you enter for that first placeholder will be duplicated into all following ones:

    for(${1:i} = ${2:0}; ${1} < ${3:count}; ${1}++)
    {
        $0
    }

# Variables

Variables allow you to insert information from the editor ([[Text Clip Variables]]) or system environment into the text you generate:

    /**
     * @file ${PN_FILENAME}
     * /

When you're not sure that a variable will have a value, you can include an optional default:

    Selection: ${PN_CURRENTWORD:(none)}

# Python Scripts

As of PN 2.1.5/PyPN 0.12 Text clips can contain PyPN code inside of backticks(``). Anything sent to stdout is captured and placed inside the output of the clip.

  <tag>
  `print "next line in tag"`
  </tag>

results in:

  <tag>
  next line in tag
  </tag>
  
See the [Text Clips with PyPN]({{ site.url }}/text_clip_with_pypn) page for more information and example, and see the [PyPN API]({{ site.url }}/pypn_api_pages) page for information on available functions.

## Details

The Text Clips that Programmer's Notepad comes with are stored in the Programmer's Notepad\Clips directory in sub-directories named for each Scheme. One .clips file can contain a number of text clips. When you modify or add clips of your own these are stored in the User Settings directory. See [Settings Storage]({{ site.url }}/settings_storage) for how to find these.
