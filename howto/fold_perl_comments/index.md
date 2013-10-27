---
layout: up
title: Folding Perl Comments
---

# Changing the Perl scheme to have folding comments
Locate perl.scheme at Programmer's Notepad\schemes folder and edit it. Search for the line that starts the language declarations:

```xml
<language name="perl" title="Perl" folding="true">
```

and add the following two values just before the right bracket:

`foldcomments="true" foldcompact="true"`

Save perl.scheme. Now, when you edit a file declared as Perl, consecutive lines of comments (starting with #) can be folded to show just the first one.
