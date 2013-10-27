---
layout: up
title: Regular Expressions
---

# Search Patterns

_NOTE: For older versions of PN the tagged expressions start `\(` and end `\)` and there are no non-capture groups nor the backslash groups. _

Regular Expressions allow complicated and flexible search/replace using a specific syntax.

_Note_: Multi-line expressions (involving \n, \r, etc) are not yet supported. See Restrictions below.
## Basic Expressions


| Pattern | Meaning |
| --- | --- |
| `.` | Matches any character except new line (\n). Note: That this means "." will also match \r which might cause some confusion when you are editing a file with both \r and \n. To match all characters including new lines you can use [\s\S] (whitespace or non-whitespace). |
| `(...)` | This marks a region for tagging a match. These tag can be access using the syntax \1 for the first tag, \2 for the second, and \3 \4 ... \9. These tags can be used within the current regular expression or in the replacement string in a search/replace. |
| `|` | Alternate matches. "Or" <nowiki>'A|B'</nowiki> will match an 'A' or an 'B' |
| `\1, \2, etc` | This refers to the first through ninth (\1 to \9) tagged region when replacing. For example, if the search string was Fred\([1-9]\)XXX and the replace string was Sam\1YYY, when applied to Fred2XXX this would generate Sam2YYY. **Note**: As only 9 regions can be used you can safely use replace string \10\2 to produce "text from region 1"0"text from region 2". |
| `[...]` | This indicates a set of characters, for example, [abc] means any of the characters a, b or c. You can also use ranges, for example [a-z] for any lower case character. |
| `[^...]` | The complement of the characters in the set. For example, `[^A-Za-z]` means any character except an alphabetic character. |
| `^` | This matches the start of a line (unless used inside a set, see above). |
| `$` | This matches the end of a line. |
| `*` | This matches 0 or more times. For example, Sa*m matches Sm, Sam, Saam, Saaam and so on. |
| `+` | This matches 1 or more times. For example, Sa+m matches Sam, Saam, Saaam and so on. |
| `?` | This matches 0 or 1 occurences. For example, Sa?m matches Sm, Sam. |
| `{n}` | This matches exactly n times. For example, 'Sa{2}m' matches Saam. |
| `{m,n}` | This matches at least m times at most n times (if n is excluded then any number of times). For example, 'Sa{2,3}m' matches Saam or Saaam. 'Sa{2,}m' is the same as 'Saa+m'|
| `*?, +?, ??, {n,m}?` | non-greedy matches -- matches the first valid match. Normally '<.*>' will match the whole string '<tag>content</tag>' -- but '<.*?>' will match '<tag>' and '</tag>'. |
 | This marks a region for tagging a match. These tag can be access using the syntax \1 for the first tag, \2 for the second, and \3 \4 ... \9. These tags can be used within the current regular expression or in the replacement string in a search/replace. |
| 

## Tagging and Groups

| Pattern | Meaning |
| --- | --- |
| `(...)` | A capture group. Accessable though \1 for the first group, \2 for the second and so on. |
| `(?:...)` | A Non-capture group. |
| `(?=...)` | Non-capture group -- Look ahead assertion. '(.*)(?=ton)' given 'Appleton' will match 'Apple'. |
| `(?<=...)` | Non-capture group -- Look behind assertion. '(?<=sir) (.*)' given 'sir William' will find ' William'.  |
| `(?!...)` | Non-capture group -- negative look ahead assertion. '.(?!e)' given 'Apple' will find each letter with the exception of 'l' because it is followed by an 'e'. |
| `(?<!...)` | Non-capture group -- negative look behind assertion. '(?<!sir) (.*)(?=ton)' given 'sir William' will find ' William'.  |
| `(?P<name>...)` | Named capture group. Assign a name to a group for later use: <nowiki>'(?P<first>A[^\s]+)\s(?P=first)'</nowiki> will find 'Apple Apple'. Similar to <nowiki>'(A[^\s]+)\s\1'</nowiki> but uses names rather than group number. |
| `(?=name)` | Match to named group. see (?P<name>...) for example. |
| `(?#comment)` | Comment -- contents of the parentheses are ignored during matching. |

## Special Symbols

| Pattern | Meaning |
| --- | --- |
| `\s` | Match whitespace.  _note: will match the end of like marker. Use `[[:blank:]]` when you need to avoid matching to a newline character._ |
| `\S` | Match non-whitespace |
| `\w` | Match word character |
| `\W` | Match non-word character |
| `\d` | Match numeric digit |
| `\D` | Match non-digit character |
| `\b` | Match word boundary. '\bW\w+' -- finds words that begin with a 'W' |
| `\B` | Match non-word boundary. '\Be\B+' -- finds the letter 'e' only when it is in the middle of a word |
| `\<` | This matches the start of a word using Scintilla's definitions of words. |
| `\>` | This matches the end of a word using Scintilla's definition of words. |
| `\x` | This allows you to use a character x that would otherwise have a special meaning. For example, \[ would be interpreted as [ and not as the start of a character set. |

## Character Classes

| Pattern | Meaning |
| --- | --- |
| `[[:alpha:]]` | Match a letter character: [A-Za-z] |
| `[[:digit:]]` | Match a digit character: [0-9] |
| `[[:xdigit:]]` | Match a hexadecimal digit character: [0-9A-Fa-f] |
| `[[:alnum:]]` | Match an alphanumeric character: [0-9A-Za-z] |
| `[[:lower:]]` | Match a lower case character: [a-z] |
| `[[:upper:]]` | Match an upper case character: [A-Z] |
| `[[:blank:]]` | Match a blank (space or tab):[ \t] |
| `[[:space:]]` | Match a whitespace  character):[ \t\r\n\v\f] |
| `[[:punct:]]` | Match a punctuation character: [-!"#$%&'()*+,./:;<=>?@\[\\\]_`{|}~] |
| `[[:graph:]]` | Match Graphical character: [\x21-\x7E] |
| `[[:print:]]` | Match Printable character (graphical characters and spaces) |
| `[[:cntrl:]]` | Match control character |


# Replacing
Regular Expressions supports `tagged expressions`. This is accomplished using `(` and `)` to surround the text you want `tagged`, and then using `\1` in the replace string to substitute the first matched text, `\2` for the second, etc.

For example:

| Text body | Search string | Replace string | Result |
| --- | --- | --- | --- |
| Hi my name is Fred | `my name is (.+)` | `my name is not \1` | Hi my name is not Fred |
| The quick brown fox jumped over the fat lazy dog | `brown (.+) jumped over the (.+)` | `brown \2 jumped over the \1` | The quick brown fat jumped over the fox lazy dog |

# Restrictions
Support for regular expressions in PN2 is currently limited, the supported patterns and syntax are a very small subset of the powerful expressions supported by perl. The biggest restriction is that regular expressions match only within a single line, you cannot use multi-line regular expressions.  As a workaround to the lack of multi-line search, you can instead use BackslashExpressions.

There are plans to improve this support by using the PCRE library (used elsewhere in PN2) to provide document searching. If you're interested in helping please make yourself known to the pn-discuss mailing list: PN Mailing Lists.



# Examples

| Description | Search | Replace |
| --- | --- | --- | --- |
| Remove leading whitespace on each line | `^[ \t]*` | |
| Change getVariable() to setVariable() | `get(\w+)\(\)` | `set\1()` |

Breaking up a URL to display arguments:
Given a URL such as `http://www.google.com/search?q=Programmers+Notepad&ie=utf-8&oe=utf-8&aq=t&rls=org.mozilla:en-US:official&client=firefox-a`

| Description | Search | Replace |
| --- | --- | --- |
| Breakup a URL's arguments | `.*?[?&](\w+)=([\w-+%.:]+)(.*)$` | `\1: \2\n\3`  |

Place cursor at the beginning of the line. Hit the FindNext button, and then repeatedly hit the Replace button. Note that hitting "replace all" will not work right since we want to search though the results of the last replace.

Result is a list of the URL parameters:

```
q: Programmers+Notepad
ie: utf-8
oe: utf-8
aq: t
rls: org.mozilla:en-US:official
client: firefox-a
```

# See Also
 * [Backslash Expressions]({{ site.url }}/search/backslash_expressions/) - very limited search/replace syntax, but supports multi-line searches.
