---
layout: up
title: Backslash Expressions
---

# Backslash Expressions

Backslash Expressions are a simple substitute for [Regular Expressions]({{ site.url }}/search/regular_expressions) that work in some simple use cases.

Note: Backslash Expressions and Regular Expressions should not be used together, as one will override the other.

## Syntax

| Text | Replacement |
| --- | --- |
| `\t` | Tab |
| `\r` | Carriage-return (CR) |
| `\n` | Line-feed (LF), aka unix-style line ending |
| `\r\n` | CR+LF, aka windows-style line ending |

## Examples

| Description | Text | Search | Replace | Result |
| --- | --- | --- | --- | --- |
| Turn unix-style line endings to windows-style (LF->CRLF) | Line 1 ''LF''\\ Line2 ''LF''| `\n` | `\r\n` | Line 1 ''CRLF''\\ Line 2 ''CRLF'' |
| Replace two blank lines with one (windows-style line endings) | Line 1\\ \\ \\ Line 4 | `\r\n\r\n` | `\r\n` | Line 1\\ Line 4 |

## See Also
  * [Regular Expressions]({{ site.url }}/search/regular_expressions/) - more powerful search/replace syntax
