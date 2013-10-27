---
layout: up
title: Add Support for Your Language
---
# .scheme files

These define the fonts, colours, styles and keywords for languages that Programmer's Notepad (via Scintilla) has built-in support for. PN contains code that parses/lexes and understands a lot of languages already. These files surface and configure that support.

## Scheme element
Document element for scheme.

Child elements:

  * [style-classes](#style-classes element) ?
  * [keyword-classes](#keyword-classes element) ?
  * [base-language](#base-language element) *
  * [language](#language element) +

## style-classes element
Container element for style classes.

Child elements:

  * [style-class](#style-class element) +

## style-class element
Purpose unknown, probably defines style settings to be inherited by other [style elements](#style element). A scheme can override predefined styles which can be found in master.scheme. All styles inherit the `default` style class, `default` style class inherits system defaults.

Attributes:

  * **name**. Identifier.
  * **description**. String. User-friendly description.
  * **inherit-style**. Identifier.
  * Other visual style settings applicable to [style element](#style element).

## keyword-classes element
Container element for keyword classes.

Child elements:

  * [keyword-class](#keyword-class element) +

## keyword-class element
Defines named keyword group.

Attributes:

  * **name**. Identifier.

Child elements:

  * [include-class](#include-class element) *

Text content:

  * whitespace-separated list of keywords.

## include-class element
Adds a keyword class to this class.

Attributes:

  * **name**. Identifier of keyword class being included.

## base-language element
Defines language settings to be inherited by [language elements](#language element).

## language element
Defines language settings.

Attributes:

  * **name**. Identifier.
  * **base**. Identifier of the [base language](#base-language element) to inherit settings from. Optional.
  * **title**. String. User-friendly language name to be presented to the user.
  * **folding**. Bool. Optional.
  * **foldcomments**. Bool. Optional.
  * **foldelse**. Bool. Optional. Fold ''if'' and ''else'' blocks as two separate blocks.
  * **foldcompact**. Bool. Optional. Include blank lines after the block being folded into the folding region.
  * **foldpreproc**. Bool. Optional.
  * **wordchars**. String. Optional. Example: wordchars="abcdeABCDE0123456789_".

Child elements:

  * [comments](#comments element) ?
  * [lexer](#lexer element) ?
  * [property](#property element) *
  * [use-keywords](#use-keywords element) ?
  * [use-styles](#use-styles element) ?

## comments element
Defines comment delimiters. This doesn't affect how the lexer highlights comments, it only affects PN functions of commenting and uncommenting blocks of code.

Attributes:

  * **line**. String. Optional. %%For C++: line="//".%%
  * **streamStart**. String. Optional. %%For C++: streamStart="/*".%%
  * **streamEnd**. String. Optional. %%For C++: streamEnd="*/".%%
  * **blockStart**. String. Optional. %%For C++: blockStart="/**".%%
  * **blockLine**. String. Optional. %%For C++: blockLine=" *".%%
  * **blockEnd**. String. Optional. %%For C++: blockEnd=" */".%%

## lexer element
Assigns lexer for the language.

Attributes:

  * **name**. Identifier of lexer to use. For .scheme file this is an identifier of a built-in scintilla lexer. This affects, how the text can be highlighted, how many keyword lists and styles can be used.
  * **stylebits**. String. Optional. Purpose unknown.

## property element
Key-value pair for lexer [property](http://www.scintilla.org/ScintillaDoc.html#SCI_SETPROPERTY).

Attributes:

  * **name**. Identifier of lexer property.
  * **value**. String. Property value to set.

## use-keywords element
Container element for keywords used by the language.

Child elements:

  * [keyword](#keyword element) +

## keyword element
Defines binding between keyword class and keyword list index.

Attributes:

  * **name**. String. User-fiendly name for the keyword class.
  * **key**. Zero-based index of corresponding WordList as seen by lexer. Lexer can assign a special meaning to a particular word list, for example, C++ lexer uses word list 2 for doxygen keywords to be highlighted in documentation comments.
  * **class**. Identifier. Name of keyword class to bind.

## use-styles element
Container element for styles used by the language.

Child elements:

  * [style](#style element) *
  * [group](#group element) *

## style element
Defines style settings.

Attributes:

  * **name**. String. User-fiendly name for the style.
  * **key**. Number. Number of corresponding style as seen by lexer. Style keys are lexer-specific. For concrete values see SCE_ definitions in [SciLexer.h](http://scintilla.cvs.sourceforge.net/viewvc/scintilla/scintilla/include/SciLexer.h?view=markup) for the lexer of interest. For their meaning see the lexer source [LexLLL.cxx file](http://scintilla.cvs.sourceforge.net/viewvc/scintilla/scintilla/src/), ColouriseLLLDoc function). See also [scintilla docs on style numbers](http://www.scintilla.org/ScintillaDoc.html#StyleDefinition).
  * **class**. Identifier. Optional. Purpose unknown, probably name of default or custom [style class](#style-class element) to inherit settings from.
  * **fore**. Color. Optional.
  * **back**. Color. Optional.
  * **bold**. Bool. Optional.
  * **italic**. Bool. Optional.
  * **font**. String. Optional. Font name.
  * **eolfilled**. Bool. Optional. Determines whether the style's background will extend past end of line.

## group element
Groups styles. Purpose unknown, probably defines common style settings for enclosed styles.

Attributes:

  * **name**. String. User-fiendly name for the style.
  * **description**. String. Optional.
  * **class**. Identifier. Optional. Purpose unknown, probably identifies a [style class](#style-class element) to inherit style settings from.

Child elements:

  * [style](#style element) *

### .schemedef files
 
These define the syntax for a language as well as the fonts, colours... The files are interpreted by a plugin for PN called customscheme.dll which provides code to tell PN how to interpret text according to the rules in the .schemedef. Scintilla built-in lexers (used by .scheme files) usually highlight text according to hardcoded syntax rules, customscheme lexer supports simple configuration of the language syntax.

### key attribute
 
As PN scans the text in a file it assigns every character in the file a "style", represented by a key.
Unfortunately each different built-in language can and often does use the keys to mean different things. So in C++ style 5 is used for keywords whereas HTML uses style 5 for numbers. There is one definitive source of information mapping key numbers to meanings, and that's the .properties files shipped with the SciTE distribution - SciTE is the test editor developed alongside Scintilla.

One style number that is almost always unchanged is that 32 is the default text style - normally this means text with no specific meaning.

When using a .schemedef file, the style keys are fixed:

  * 32: Default 
  * 1: Line Comment Style 
  * 2: Block Comment Style 
  * 3: First Identifiers Type 
  * 4: Numbers 
  * 5: Keywords 
  * 6: Keywords 2 
  * 7: Keywords 3 
  * 8: Keywords 4 
  * 9: Keywords 5 
  * 10: First String Type 
  * 11: Second String Type 
  * 12: Pre-processor 
  * 13: Second Identifiers Type 

### Keywords

The limitation to keyword sets is defined by two hard limits: The first is that Scintilla supports a fixed maximum number of keyword sets, currently 8. The second is that each language lexer only uses the keyword sets that the author thought were necessary - this means that often a language will support only one or two sets of keywords. The .schemedef lexer supports 5 as when it was developed that was the Scintilla limit. 

### User Configuration and Caches

The changes a user makes to fonts and colours are not stored in the original .scheme or .schemedef files, the configuration of these things is instead stored in a file called UserSettings.xml in the per-user PN application data directory. Unless you're using the portable edition of PN, this will likely be here:

Vista: c:\users\[your username]\AppData\Roaming\Echo Software\PN2

XP: c:\documents and settings\[your username]\Application Data\Echo Software\PN2

The configuration in usersettings.xml is applied to the schemes you have installed, and the result is cached in a binary format in the .cscheme files you'll find in that directory.