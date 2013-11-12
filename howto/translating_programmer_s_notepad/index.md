---
layout: up
title: Translating Programmer's Notepad
---

# Translating Programmer's Notepad

Programmer's Notepad comes with a number of community created translations, and new contributions are welcome! The translations use the standard PO format files, which can be edited by a wide variety of tools. We recommend using [Poedit](http://www.poedit.net/).

## Steps to add a new language:

  - Download the current language translation kit: [PN Translation Kit](https://github.com/simonsteele/pn/releases/download/v2.4/translatetools2402378.zip)
  - Create a new catalog (.po file) from the English template file, translations\pnlang_2057_en-GB.pot (in Poedit go to File -> New Catalog from POT file...)
  - Fill in all the translations, ignoring any starting with "ID:"
  - Save the catalog and do one of:
    - upload your change as an issue on the [github project](https://github.com/simonsteele/pn/issues)
    - fork the project on github and submit a pull request with your change

When saving your translation, it should be saved with a name conforming to this format:

```
pnlang_<lcid>_<langcode>.po
```

The lcid is the decimal language code ID for your language, and langcode is the short ISO representation for the language. Good tables to find these are here: [Locale ID Chart on MSDN](http://msdn.microsoft.com/en-us/library/ms912047%28WinEmbedded.10%29.aspx), [LCID Structure](http://msdn.microsoft.com/en-us/library/cc233968%28v=PROT.10%29.aspx)

The language will be made available with the next PN release, but will need updating for each new release. Testing releases will not be delayed for language updates, but Stable releases may be.

## Converting a PO file to a Language DLL:

This allows you to test your translation with Programmer's Notepad.

  - Run this command: `tools\ResText\ResText apply bin\pnlang.dll bin\pnlang_<language details>_<version>.dll translations\<your translation>.po`
  - Copy the new DLL file to your Programmer's Notepad directory and then start PN.
  - In options select your language and restart

## Updating the template file

The template file is updated for each translation tools release, but if you wish to do this yourself you'll need to run this command from the translation tools directory:

```
tools\ResText\ResText extract bin\pnlang.dll translations\pnlang_2057_en-GB.pot
```

## Merging the latest strings from the template to your language

For this to work you'll need the msgmerge tool, I use the one bundled with the PoEdit tool. Here are the steps:

  - Run this: `msgmerge -o temp.po translations\<your translation>.po translations\pnlang_2057_en-GB.pot`
  - Copy temp.po over your original translation file if you're happy with the result

