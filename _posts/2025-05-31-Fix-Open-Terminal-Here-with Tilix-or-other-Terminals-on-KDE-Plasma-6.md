---
layout: post
title: Fix "Open Terminal Here" with Tilix or other Terminals on KDE Plasma 6 (Fedora 42)
date: 2025-05-30
description: From the browser to the terminal in just two clicks
tag: computers
---
### Introduction
After uninstalling Konsole and switching to Tilix on KDE Plasma 6 (Fedora 42), I found that right-clicking and choosing "Open Terminal Here" - whether on the desktop or in Dolphin - did not open anything, even after setting the default terminal in KDE's system settings. You can also use this with any other terminal of your choice.

Here's a simple way to fix it by creating a custom service menu that launches Tilix:

## Steps

1. Create a service menu file:

```bash
mkdir -p ~/.local/share/kio/servicemenus
nano ~/.local/share/kio/servicemenus/tilixhere.desktop
```

2. Paste this content into the file:

```ini
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=inode/directory;
Actions=openTilixHere
X-KDE-Priority=TopLevel

[Desktop Action openTilixHere]
Name=Open Tilix Here
Icon=tilix
Exec=tilix --working-directory %f
```

3. Save and close the file.

4. Restart Dolphin or your desktop shell to apply the change:

```bash
killall dolphin && dolphin &
```

or log out and back in.

## Optional: Set Tilix as the default terminal globally

Run:

```bash
kwriteconfig6 --file kdeglobals --group General --key TerminalApplication tilix
```

Then restart Plasma:

```bash
kquitapp6 plasmashell && kstart plasmashell
```

(or use `kstart` if `kstart6` is not available)

---

This makes sure KDE uses Tilix for "Open Terminal Here" actions system-wide.

Tested on Fedora 42 with KDE Plasma 6 and Tilix 1.9.
