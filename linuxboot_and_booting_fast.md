% LinuxBoot and booting fast
% Paul Menzel (Max Planck Institute for Molecular Genetics)
% 28. September 2018

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

# Motivation

## Famous words.

> We do not trust or like firmware.

1.  Slow
1.  Buggy
1.  Pain to update
1.  Often proprietary
1.  Different code base

# LinuxBoot

## Motivation

1.  Gegründet von Ron Minnich als LinuxBIOS am [LANL](https://www.lanl.gov/)
1.  Konfiguration mit Tastatur und Aktualisierung der Firmware von über 1.000 Clusterknoten mit DOS-Programmen inakzeptabel
1.  [„Press F1 to continue.“](http://www.h-online.com/open/features/The-Open-Source-BIOS-is-Ten-An-interview-with-the-coreboot-developers-746525.html)
1.  [The Linux BIOS](https://www.coreboot.org/images/1/14/Linuxbios.ps)
1.  [Who is working on coreboot?](https://www.coreboot.org/FAQ#Who_is_working_on_coreboot.3F)
1.  [https://www.coreboot.org/Clusters](SC 2000: The first LinuxBIOS cluster, built at SC 2000, now at LANL)
1.  [1024-node linuxbios cluster with Dual-P4 systems and Myrinet](https://mail.coreboot.org/pipermail/coreboot/2002-September/000297.html)

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
