---
layout: post
title: "Disabling Consistent Network Device Naming in RHEL and CentOS"
description: "Returning to the wonderful land of ethX"
tag: Computers
---

Consistent Network Device Naming was concieved to mitigate issues that would arise on machines where new network interfaces were installed, thus possibly changing the boot order, and their corresponding network device name [1]. Whilst this is incredibly useful in many scenarios, there remains certain situations where this is undesirable, and can cause many issues. Red Hat introduced this in RHEL7 and CentOS7[2], but thankfully this is relatively easy, albeit slightly hazardous to fix.

### The Fix
You need to edit the kernel boot arguments, to do so, open the grub.cfg file.
```Shell
vi /etc/default/grub
```
This should appear similar to the following:
```vim
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos_mysystem/root 
rd.lvm.lv=centos_mysystem/swap rhgb quiet"
GRUB_DISABLE_RECOVERY="true"
```
Two arguments, biosdevname=0 and net.ifnames=0, need to be added to the sixth line beginning with "GRUB_CMDLINE_LINX", to make it appear like the following:

```vim
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos_genserver01/root 
rd.lvm.lv=centos_genserver01/swap rhgb quiet biosdevname=0 net.ifnames=0"
```
After this is done, exit your editor and make the grub2 config, and reboot:

```vim	
grub2-mkconfig -o /boot/grub2/grub.cfg
```

```Shell
reboot
```

Run ifconfig (net-tools is required) and confirm that the ethX naming scheme is running:

```Shell
ifconfig
```
Whilst we are almost done, we still need to edit the network scripts:

```Shell	
cd /etc/syconfig/network-scripts
ls -l
```

There are many files in this folder, but as you will notice, there is no ifcfg-eth0, only ifcfg-enp0s3 (or similar). This file needs to be removed, but it is good to place it with the temopoary files in case it is required for reference or an error arises. 

```Shell
mv ifcfg-enp0s3 /tmp/
```

Now a new file needs to be created.
	
```Shell
vi ifcfg-eth0
```

The contents of this file will vary on your requirements, but it will most probably be nearly identical to your ifcfg-enp0s3 file, with a few changes.
	
```vim
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="c4f7a4f0-3282-4aae-8a67-6282259f5467"
DEVICE="enp0s3"
ONBOOT="yes"
```

Three lines need to be changed, name and device must change to "eth0", and the UUID needs to be changed. The UUID of the new device can be found by running the following:
	
```Shell
uuidgen eth0
```

### References
[1] M. Domsch, "PATCH: Network Device Naming mechanism and policy [LWN.net]", 2009. [Online]. Available: https://lwn.net/Articles/356900/. [Accessed: 26- Sep- 2017]

[2] "Chapter 8. Consistent Network Device Naming - Red Hat Customer Portal", Access.redhat.com, 2017. [Online]. Available: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/ch-consistent_network_device_naming. [Accessed: 26- Sep- 2017]
