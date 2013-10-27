---
layout: up
title: Automatic Indentation
---

# Automatic Indentation

After you've made your .scheme or .schemedef file (see [Add Support for Your Language]({{ site.url }}/howto/add_support_for_your_language)) there's still something missing from that authentic _professional_ feeling when programming with your newly defined language. For that you need to create an auto-indenter.

Please note: You need to have **PyPN** installed for this to work! See [Install PyPN]({{ site.url }}/howto/install_pypn) for further instructions.

## The Essentials

To create an auto-indenter you must make a new Python script that PN recognizes as an indenter. Throughout this tutorial I'm going to use _The Elder Scrolls_ scripting language for _Oblivion_ as an example (_here should be tes.schemedef file but the site don't allow me to upload it_). More information about it can be found at http:_cs.elderscrolls.com/constwiki/index.php/Portal:Scripting.

Every auto-indenter must import at least the following (in most cases these are enough too):

```python
import scintilla
from pypn.decorators import indenter
```

After the imports we define the indenter function. Let's see the start of that definition from the example before further explanation:

```python
@indenter("tes")
def tes_indent(c, doc):
```

The first line tells PyPN that the following function is meant to be an indenter. The string in parentheses must match the name of the scheme or schemedef of the language we're trying to indent. The second line is regular Python. You can name the function what ever you like but descriptive names like ''tes_indent'' have advantages. The function must have two parameters - their naming doesn't matter but as PyPN already uses ''c'' and ''doc'' for them there's no reason to change.

## Indenting by Keywords

In _Oblivion_ scripts the indentation is made by keywords. There are few keywords that start a block that gets indented and there are other keywords that end such blocks and cause unindentation. Easiest way to define them in Python is to use a list. I find it best to define these outside of the function.

```python
# Keywords that cause indentation.
i_kws = ['scriptname', 'scn', 'begin', 'if', 'elseif']
# Keywords that cause unindentation.
u_kws = ['end', 'endif']
```

Lines starting with ''#'' are Python comments. Variable names are again inconsequential - just use what you consider descriptive. In Python lists are surrounded by brackets and list items are separated by commas. As _Oblivion_ scripts are not case-sensitive it's easiest to type the keywords in lowercase.

People like different tabwidths for indentation and such preferences often vary from language to language. To ease that we define one more variable before we move on to function itself (I've used tabwidth of two because that's how most of the code in their Wiki is indented but in most cases I'd prefer four):

```python
# Tabwidth used for indentation.
tab = 2
```

## Inside the Function: Local Variables

To be able to auto-indent we need quite a bit of data from our document. Depending on how your language is indented you may need more or less than we do in our example but you should get the idea. Let's have the next bit of code before further explanation (I've included some of the stuff from earlier so that you know where to place this):

```python
@indenter("tes")
def tes_indent(c, doc):
    sci = scintilla.Scintilla(doc)
    pos = sci.CurrentPos
    l_cur = sci.LineFromPosition(pos)
    t_start = sci.PositionFromLine(l_cur - 1)
    t_end = pos - 1
    txt = sci.GetText(t_start, t_end)
    kw = txt.split()[0].lower()
```

In almost every PyPN script we a have variable that is set to ''scintilla.Scintilla(doc)'' (or more often actually ''scintilla.Scintilla(pn.CurrentDoc())''). We need it to use Scintilla's functions through PyPN.

For our needs we need to know the following about our document:

  * Current location of the caret (''sci.CurrentPos'')
  * Current line (''sci.LineFromPosition(pos)'')
  * Location of the start of previous line (''sci.PositionFromLine(l_cur - 1)'')
  * Location of the end of previous line (''pos - 1'')
  * Contents of the previous line (''sci.GetText(t_start, t_end)'')
  * First word on previous line in lowercase (''txt.split()[0].lower()'')

With this information at hand we can move on.

## Inside the Function: Functionality

First, let's figure out what we're trying to do. Here's the logic in pseudo language:

```python
if [first word on previous line] == [a block starting keyword]
    -> indent current line
elseif [first word on previous line] == [a block ending keyword]
    -> unindent previous and current line
```

So obviously we're going to need some if statements. Let's see some code before further explanation:

```python
    if kw in i_kws:
        c_ind = sci.GetLineIndentation(l_cur)
        p_ind = sci.GetLineIndentation(l_cur-1)
        if c_ind == p_ind or c_ind == 0:
            c_ind += tab
            sci.IndentLine(l_cur, c_ind)
```

This is the if clause for indentation. First we check if the word ''kw'' is among those that start an indented block. Then we get the indentation levels for current and previous line with ''sci.GetLineIndentation()'' and check that either both lines are equally indented or that the current line is not indented at all. If either of these is correct current line is indented by the amount defined in our variable ''tab''.

```python
    elif kw in u_kws:
        c_ind = sci.GetLineIndentation(l_cur)
        p_ind = sci.GetLineIndentation(l_cur-2)
        if c_ind == p_ind:
            c_ind -= tab
            sci.IndentLine(l_cur, c_ind)
            sci.IndentLine(l_cur-1, c_ind)
```

Code for unindentation is very similar. Most notable differences are that we unindent both current and previous line, and due to that we use the line before previous when comparing indentation levels of lines (otherwise we'd unindent twice on Windows where EOL is marked by ''\n\r''). Also ''kw'' is obviously searched from the list containing block ending keywords.

## Complete Script

```python
import scintilla
from pypn.decorators import indenter

# Keywords that cause indentation.
i_kws = ['scriptname', 'scn', 'begin', 'if', 'elseif']
# Keywords that cause unindentation.
u_kws = ['end', 'endif']
# Tabwidth used for indentation.
tab = 2

@indenter("tes")
def tes_indent(c, doc):
    sci = scintilla.Scintilla(doc)
    pos = sci.CurrentPos
    l_cur = sci.LineFromPosition(pos)
    t_start = sci.PositionFromLine(l_cur - 1)
    t_end = pos - 1
    txt = sci.GetText(t_start, t_end)
    kw = txt.split()[0].lower()
    
    if kw in i_kws:
        c_ind = sci.GetLineIndentation(l_cur)
        p_ind = sci.GetLineIndentation(l_cur-1)
        if c_ind == p_ind or c_ind == 0:
            c_ind += tab
            sci.IndentLine(l_cur, c_ind)
    elif kw in u_kws:
        c_ind = sci.GetLineIndentation(l_cur)
        p_ind = sci.GetLineIndentation(l_cur-2)
        if c_ind == p_ind:
            c_ind -= tab
            sci.IndentLine(l_cur, c_ind)
            sci.IndentLine(l_cur-1, c_ind)

```

## Final Words

With the current lack of documentation and some bugs in PyPN creating scripts is often about learning from errors. I used the Python indenter that comes with PyPN as a some sort of a starting point (I must notify that there is a redundant piece of code in that one - there is no need to check if the character is EOL as that's already done in ''pypn.glue''). PN source code and Scintilla documentation were also essential. I hope someone finds this useful.