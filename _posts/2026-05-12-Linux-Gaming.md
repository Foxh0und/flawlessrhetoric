---
layout: post
title: Linux Gaming
date: 2026-05-12
description: Migrating from Windows in 2026
tag: Computers
---

Roughly six months ago, I started gaming on Linux. I'd read several articles about it, heard lots about Proton, and several friends were now doing it, completely dodging Windows. I've been dual booting since 2017 when I [built my first Ryzen based PC](https://www.flawlessrhetoric.com/Ryzen), and my most recent build in 2023 is the same, Fedora and Windows 10. I use Fedora for everything bar gaming, and Windows for only that. Dual booting can get annoying, but it's worked really well for the most part.

With Microsoft announcing more and more Copilot integrations, everything becoming a subscription, and my PC feeling like one big advertisement I decided to make a change. It was relatively straight forward, I did some searching, and made some changes to my machine (detailed below), and installed Steam. I didn't blow my Windows drive away, and kept it in case I 

I was currently playing God of War: Raganarok, and installed that. I had anticipated that being a Playstation game, albeit a near flawless port, it would be problematic, but this was far from the case. The game is marked 'Gold' on (ProtonDB)[https://www.protondb.com/app/2322010], meaning that it 'runs perfectly well after tweaks'. After adding a single line to the launch options, it ran perfectly, with no discernable difference to Windows. I have since played Team Fortress 2 (a native Windows game) and HellDivers 2, both of which were fine. This week I purchased Stalker 2: Heart of Chernobyl (marked Platinum), and despite all it's well documented bugs runs perfectly. 

The only time the machine needed tinkering was with the fan profiles which were running too loud. I now manage that simply through [LACT](https://github.com/ilya-zlobintsev/LACT). Using an AMD GPU is also much easier with Linux, having everything come out of the box. I'm really looking forward what Proton 11 (bringing the [vast improvements that Wine 11 accomplished](https://www.xda-developers.com/wine-11-rewrites-linux-runs-windows-games-speed-gains/)) can do.

Moving to a single OS has been a godsend, I no longer need to reboot to change tasks and have everything running on one machine. It is so much better. I could even game, then switch to writing this! If I miss out, or have to wait on a new game because it doesn't run on Linux then so be it, there are plenty of other games out there to play. 

#### **Hardware Specs**

* **CPU:** AMD Ryzen 7 7800X3D
* **GPU:** AMD Radeon RX 7900 XT (Navi 31, RADV driver)
* **Memory:** 32 GB
* **OS:** Fedora Linux 42 (KDE Plasma)
* **Driver stack:** Mesa RADV, Vulkan
* **Game platform:** Steam (Flatpak / native)

## Changes Made

### **1. Ensuring the Correct GPU Was Used**

I verified Vulkan was correctly detecting the discrete GPU using:

```
vulkaninfo | grep -i radv
```

This showed two devices:

* GPU 0: AMD Radeon RX 7900 XT (correct)
* GPU 1: AMD Ryzen 7 7800X3D iGPU (unused)

Steam + Proton automatically use GPU 0 through RADV.

---

### **2. Installing and Enabling GameMode**

GameMode boosts CPU scheduling, I/O priority, and switches the system to the performance power profile *only while gaming*.

##### Installation:

```
sudo dnf install gamemode
```

#### Global Steam integration:

I added this to Steam's global launch options:

```
gamemoderun %command%
```

I also learned that GameMode temporarily sets the system to **performance**, so the base system profile should stay on **balanced**:

```
powerprofilesctl set balanced
```

---

### **3. Monitoring Temps with MangoHud**

To check GPU load, temps, and performance during gameplay, I use MangoHud:

```
sudo dnf install mangohud
```

Steam launch option:

```
mangohud gamemoderun %command%
```

This overlay confirms:

* RX 7900 XT is used
* FPS
* GameMode status
* GPU junction/mem temps

---

### **4. Using LACT for Quiet Gaming**

I wanted quieter gaming, so I switched from pure performance tuning to thermal/noise optimisation using **LACT** (Linux AMD Control Tool).

LACT lets me:

* Adjust GPU power limit (PPT)
* Set fan curves
* Apply undervolting

The RX 7900 XT defaults around 285–300W. Lowering this slightly reduces noise dramatically.

Example adjustments I used:

* **PPT limit:** ~260–285W
* Slight undervolt (e.g. -30 to -50mV)
* Custom fan curve with smoother ramp-up

This kept junction temps in sane ranges:

* Junction: ~85–90°C
* Memory: ~75–82°C
* Edge: 55–65°C

---

### **5. Proton (10), Proton Experimental, and Proton-GE**

I experimented with different Proton versions.

##### Proton Experimental

* Highest performance
* Sometimes unstable
* Can cause FPS spikes → higher temps

##### Proton 10 (stable)

* Most consistent
* Lower chance of bugs
* Slightly cooler because FPS/power behaviour is more predictable

##### Proton-GE

* Great for compatibility
* Often smoother frame pacing → *reduced GPU power spikes*
* Slightly quieter gaming compared to Experimental

I switched from **Proton Experimental** → **Proton 10** for stability and cooler behaviour.

---

### **6. Temperature Analysis with `sensors`**

Using `sensors`, I monitored:

* GPU edge, junction, memory temps
* Fan RPM
* PPT usage
* CPU temperatures

Example output showed:

* Junction up to ~90°C (normal for RDNA3)
* Memory ~80°C
* GPU fans ~2000 RPM
* PPT ~286W (default)

After LACT tuning, noise was significantly lower.

---

### **7. Final Configuration**

#### System profile:

```
powerprofilesctl set balanced
```

#### Steam global launch options:

```
mangohud gamemoderun %command%
```

#### Proton version:

Using **Proton 10** for most games.

#### LACT settings:

* Reduced PPT
* Slight undervolt
* Quiet fan curve

#### Result:

* Quieter gameplay
* Lower temps
* Smooth performance
* Stable FPS
* GPU properly utilised

---

### **Conclusion**

By combining Proton 10, GameMode, MangoHud, and LACT, I achieved a Fedora 42 gaming setup that balances performance with quiet cooling. This configuration ensures the RX 7900 XT runs efficiently without screaming fans, while still delivering strong performance in modern games.

If you're running Fedora with AMD hardware, these are some of the simplest and most effective optimisations you can apply to improve both noise and stability while gaming.