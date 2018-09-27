% LinuxBoot and booting fast
% Paul Menzel (Max Planck Institute for Molecular Genetics)
% September 28th, 2018

## Who am I?

![Logo of Max Planck Institute for Molecular Genetics](images/MPIMG_helix_rgb.png){ height=25% }\


- (Economic) Mathematician by studies at [TU Berlin](https://www.tu-berlin.de/)
- Free Software enthusiast
- Active in [coreboot](https://www.coreboot.org/) since 2005 (still LinuxBIOS back then)
- System architect at [Max Planck Institute for Molecular Genetics](https://www.molgen.mpg.de/)

## Presentation

Used Markdown and Pandoc, sources available online.

TinyURL: <https://tinyurl.com/linuxbootfast>

<https://github.com/paulmenzel/linuxboot-and-booting-fast>

# Introduction

## Why this talk?

1.  Warning: Focus on x86
1.  Goal of fast boot
1.  Bad experiences with proprietary firmware
1.  Interesting topics
1.  Cool community

# LinuxBoot

## Motivation

> We do not trust and do not like firmware.

1.  Everywhere
1.  High privileges, and is essentially an OS
1.  Slow
1.  Buggy
1.  Pain to update
1.  Often proprietary
1.  Different and unfamiliar code base
1.  Quite limited in functionality
1.  Necessary to write to flash ROM chip for update

# Solution: LinuxBoot – Let Linux do it

## Advantages of Linux kernel

1.  Familiar code base
1.  Well tested
1.  Great hardware support
1.  Kexec as boot loader
1.  Familiar user space in initrd
1.  Fix issue by reboot without flashing something

## Implementation

1.  Make firmware as small as possible
1.  Move as much as possible into the Linux kernel
1.  Use a small Linux kernel as *boot kernel*
1.  Use Linux as bootloader with kexec

## History of LinuxBIOS

1.  Started by Ron Minnich as LinuxBIOS at [LANL](https://www.lanl.gov/)
1.  [„Press F1 to continue.“](http://www.h-online.com/open/features/The-Open-Source-BIOS-is-Ten-An-interview-with-the-coreboot-developers-746525.html)
1.  [The Linux BIOS](https://www.coreboot.org/images/1/14/Linuxbios.ps)
1.  [https://www.coreboot.org/Clusters](SC 2000: The first LinuxBIOS cluster, built at SC 2000, now at LANL)
1.  [1024-node linuxbios cluster with Dual-P4 systems and Myrinet](https://mail.coreboot.org/pipermail/coreboot/2002-September/000297.html)

## Other talk on LinuxBoot

Chris Koch: [Netboot21: Bootloaders in the 21st Century](https://cfp.all-systems-go.io/en/ASG2018/public/events/215)

> User-space bootloaders with LinuxBoot

Saturday, 4:30 p. m.

# Booting fast

## Motivation

- S4 good, but S3, really?

## Past efforts

## Platform

### firmware

### Operating system

1.  Linux kernel
2.  No initramfs

## Methods

1.  initcall_debug
2.  systemd-bootchart
3.  `bootgraph.py` with ftrace
