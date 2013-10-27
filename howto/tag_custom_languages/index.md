---
layout: up
title: Tagging Custom Languages
---
# Tagging Custom Languages 
Programmers Notepad uses [CTags](http://ctags.sourceforge.net/) to generate a list of tags for source files for supported languages. Obviously, CTags only supports a [finite set of languages](http://ctags.sourceforge.net/languages.html). Fortunately CTags allows the specification of additional languages through an external file.

CTags needs 3 things to tag a new language.

| Description | CTags Parameter | Example |
| --- | --- | --- |
| A language name. | langdef | properties |
| A list of file extensions to associate with the language. | langmap | .ini |
| A regular expression to define what to tag and what to name the tag. | regex-<NAME> | ''/^\[(.*)\]/\1/s,Section/'' |


Detailed information for the 3 parameters:

**--langdef**=<name

Defines a new user-defined language, name, to be parsed with regular expressions. Once defined, name may be used in other options taking language names. The typical use of this option is to first define the language, then map file names to it using --langmap, then specify regular expressions using --regex-<LANG> to define how its tags are found.

**--langmap**=map[,map[...]]

Controls how file names are mapped to languages (see the --list-maps option). Each comma-separated map consists of the language name (either a built-in or user-defined language), a colon, and a list of file extensions and/or file name patterns. A file extension is specified by preceding the extension with a period (e.g. ".c"). A file name pattern is specified by enclosing the pattern in parentheses (e.g. "([Mm]akefile)"). If appropriate support is available from the runtime library of your C compiler, then the file name pattern may contain the usual shell wildcards common on Unix (be sure to quote the option parameter to protect the wildcards from being expanded by the shell before being passed to ctags). You can determine if shell wildcards are available on your platform by examining the output of the --version option, which will include "+wildcards" in the compiled feature list; otherwise, the file name patterns are matched against file names using a simple textual comparison.

If the first character in a map is a plus sign, then the extensions and file name patterns in that map will be appended to the current map for that language; otherwise, the map will replace the current map. For example, to specify that only files with extensions of .c and .x are to be treated as C language files, use "--langmap=c:.c.x"; to also add files with extensions of .j as Java language files, specify "--langmap=c:.c.x,java:+.j". To map makefiles (.e.g files named either "Makefile", "makefile", or having the extension ".mak") to a language called "make", specify "--langmap=make:([Mm]akefile).mak". To map files having no extension, specify a period not followed by a non-period character (e.g. ".", "..x", ".x."). To clear the mapping for a particular language (thus inhibiting automatic generation of tags for that language), specify an empty extension list (e.g. "--langmap=fortran:"). To restore the default language mappings for all a particular language, supply the keyword "default" for the mapping. To specify restore the default language mappings for all languages, specify "--langmap=default". Note that file extensions are tested before file name patterns when inferring the language of a file.

**--regex-<LANG>**=/regexp/replacement/[kind-spec/][flags]

The /regexp/replacement/ pair define a regular expression replacement pattern, similar in style to sed substitution commands, with which to generate tags from source files mapped to the named language, <LANG>, (case-insensitive; either a built-in or user-defined language). The regular expression, regexp, defines an extended regular expression (roughly that used by egrep(1)), which is used to locate a single source line containing a tag and may specify tab characters using \t. When a matching line is found, a tag will be generated for the name defined by replacement, which generally will contain the special back-references \1 through \9 to refer to matching sub-expression groups within regexp. The '/' separator characters shown in the parameter to the option can actually be replaced by any character. Note that whichever separator character is used will have to be escaped with a backslash ('\') character wherever it is used in the parameter as something other than a separator. The regular expression defined by this option is added to the current list of regular expressions for the specified language unless the parameter is omitted, in which case the current list is cleared.

Unless modified by flags, regexp is interpreted as a Posix extended regular expression. The replacement should expand for all matching lines to a non-empty string of characters, or a warning message will be reported. An optional kind specifier for tags matching regexp may follow replacement, which will determine what kind of tag is reported in the "kind" extension field (see TAG FILE FORMAT, below). The full form of kind-spec is in the form of a single letter, a comma, a name (without spaces), a comma, a description, followed by a separator, which specify the short and long forms of the kind value and its textual description (displayed using --list-kinds). Either the kind name and/or the description may be omitted. If kind-spec is omitted, it defaults to "r,regex". Finally, flags are one or more single-letter characters having the following effect upon the interpretation of regexp:

|Flag|Effect|
|---|---|
|b|The pattern is interpreted as a Posix basic regular expression.|
|e|The pattern is interpreted as a Posix extended regular expression (default).|
|i|The regular expression is to be applied in a case-insensitive manner.|

Note: this option is available only if ctags was compiled with support for regular expressions, which depends upon your platform. You can determine if support for regular expressions is compiled in by examining the output of the --version option, which will include "+regex" in the compiled feature list.

For more information on the regular expressions used by ctags, see either the regex(5,7) man page, or the GNU info documentation for regex (e.g. "info regex").

### Example
This would cause CTags to tag section titles from .ini files.

```
--langdef=properties
--langmap=properties:.ini
--regex-properties=/^\[(.*)\]/\1/s,Section/
```

### Kinds of Tags
Note the ''s,Section'' in the regular expression above. That associates the letter ''s'' with the tag matched by the regular expression. The letter is completely arbitrary, it is only used later to associate tags with ''kinds''. These are the ''kinds'' of tags that Programmers Notepad recognizes.

```
0  Unknown
1  Function
2  Procedure
3  Class
4  Macro
5  Enum
6  Filename
7  EnumName
8  Member
9  Prototype
10 Structure
11 Typedef
12 Union
13 Variable
14 Namespace
15 Method
16 Event
17 Interface
18 Property
19 Program
20 Constant
21 Label
22 Singleton
23 Mixin
24 Module
25 Net
26 Port
27 Register
28 Task
29 Cursor
30 Record
31 SubType
32 Trigger
33 Set
34 Field
35 Table
```

### Programmers Notepad Requirements
The external file for CTags is `<pn root>\taggers\ctags\additionalLanguages.conf`. If that file exists, Programmers Notepad will pass it to CTags. If the file has syntax errors CTags might not work properly, so BE CAREFUL.

Programmers Notepad maintains a list of the schemes that support tagging, and, for each scheme, a list of the ''kinds'' of tags the scheme supports. The external file to add schemes is ''<pn root>\taggers\ctags\additionalSupportedSchemes.ini''.

Each section in this .ini file is the name a scheme (defined elsewhere in a .scheme or .schemedef file, e.g. ''<language name="properties">'').

**The name is case-sensitive.**

For each tag ''kind'' in your scheme, add a line to the section.
The key is the  letter corresponding to your tag ''kind,'' the value is ''kind'' number from the list above.

### Example
This would cause Programmers Notepad to invoke CTags for the "properties" scheme, and associate the ''s'' tags that CTags finds with "Label".

```
[properties]
s = 21
```