---
layout: post
title: Switching from Gnome to KDE on Fedora
date: 2025-05-28
description: Switch from Gnome to KDE on Fedora 42
tag: Computers
---
### Introduction
I've been contemplating moving from Gnome to KDE for a while now. I've been on Gnome for nearly 10 years. Most of my friends are on KDE, and I've been meaning to give it a try. I feel that Gnome has been lacking some features, and potentially support for a while now, and I haven't been super happy with my setup. Every time I upgrade, I always find myself having to muck around with plugins again, and I feel that it would be a pain to recreate what I had, which is still relatively standard.



I didn't want to do a clean install and manually swap to the KDE Spin. I also couldn't find any complete instructions for doing exactly what I wanted, so I wrote these. I hope they help! 

### Instructions


#### 🧰 Step 1: Install KDE

First, install the full KDE desktop environment group:

```bash
sudo dnf install @kde-desktop-environment
```

Then enable SDDM and set KDE as the default session:

```bash
sudo systemctl enable sddm --force
sudo systemctl set-default graphical.target
```

#### 🔁 Step 2: Switch Fedora Identity to KDE

Fedora uses different identity packages depending on the desktop spin. I swapped mine from GNOME Workstation to KDE:

```bash
sudo dnf swap -y fedora-release-workstation fedora-release-kde
sudo dnf swap -y fedora-release-identity-workstation fedora-release-identity-kde
```

You're also probably going to want to restart now, and boot into KDE.

#### 🧼 Step 3: Remove GNOME Packages

Start by removing the GNOME desktop group:

```bash
sudo dnf remove @gnome-desktop
```

Then remove core GNOME packages:

```bash
sudo dnf remove \
  gnome-shell gdm gnome-control-center gnome-terminal \
  --setopt=protected_packages= --setopt=protect_running_kernel=false
```

Clean up:

```bash
sudo dnf autoremove
```

And list any remaining GNOME packages:

```bash
dnf list installed | grep gnome
```

#### 🧹 Step 4: Remove Leftover GNOME Configs

Clear out user-specific GNOME settings and cached data:

```bash
rm -rf ~/.config/gnome*
rm -rf ~/.local/share/gnome*
rm -rf ~/.cache/gnome*
rm -rf ~/.config/dconf
rm -rf ~/.dbus
rm -rf ~/.themes ~/.icons ~/.fonts ~/.gtkrc*
```

#### ⚙️ Step 5: Disable GDM and Final Cleanup

Disable GDM:

```bash
sudo systemctl disable gdm.service
sudo systemctl stop gdm.service
```

Set KDE as the default desktop in `/etc/sysconfig/desktop`:

```bash
sudo nano /etc/sysconfig/desktop
```

Contents:

```ini
DESKTOP=KDE
DISPLAYMANAGER=KDE
```

Remove GNOME session entries from login menu:

```bash
sudo rm /usr/share/xsessions/gnome.desktop
sudo rm /usr/share/wayland-sessions/gnome.desktop
```

#### 🌐 Step 6: Remove GNOME Shell Integration Extension

If you used the GNOME Shell browser integration (Firefox/Chrome), remove it:

```bash
flatpak uninstall org.gnome.Extensions
```

Or remove the extension from your browser manually.

#### ✅ All Done

After a reboot, I had a clean KDE environment: no GNOME apps, no GDM, no conflicts.
I'm enjoying KDE a lot, the launcher is really nice, and the whole experience just feels that little bit better.


![Swapping Fedora release packages](/assets/images/fedorakde.png)
