---
layout: up
title: Updating Scintilla in Mercurial
---

# Updating Scintilla in Mercurial

**You need:**

  - PN tree
  - Export of latest Scintilla bits (i.e. no CVS folders)

## Steps:


In the PN tree you need to update to when the last clean Scintilla import was made, you can find this in the code browser:

```
hg update --clean [last_clean_revision]
```

Now swap in the latest scintilla bits:

```
cd third_party
move scintilla c:\temp\scintilla
move [new_scintilla_bits] scintilla
```

Now update the tree:

```
hg addremove scintilla
hg commit -m "Add scintilla release x"
```

You've now created a new mercurial head revision, but want to switch back to your main one:

```
hg heads
```

You'll see a list of heads, pick the real head (usually the revision previous to your scintilla commit) and switch to that:

```
hg update --clean 9998
```

Merge in the scintilla change:

```
  hg merge 9999
```

Check everything still builds and fix any problems, then commit:

```
  hg commit -m "Merged scintilla release x"
```