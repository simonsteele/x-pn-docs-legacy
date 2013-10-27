---
layout: up
title: Reset the Docking Windows
---

# Reset the Docking Windows

If you're using PN from an installer, please remove this registry key to reset the UI placement options:

HKEY_CURRENT_USER\Software\Echo Software\PN2\Interface Settings\default

You can also run pn --reset but this will also reset your scheme and tools config. If you are using the portable release open up the settings directory and find the Interface Settings bit in the ini file. Delete that and you will get all the windows back.