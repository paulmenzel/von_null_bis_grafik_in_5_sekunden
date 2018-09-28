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

## Power architecture (ppc64)

1.  Since Power 8 [Petitboot](https://www.kernel.org/pub/linux/kernel/people/geoff/petitboot/petitboot.html)

## Other talk on LinuxBoot

> [Netboot21: Bootloaders in the 21st Century](https://cfp.all-systems-go.io/en/ASG2018/public/events/215)
> 
> User-space bootloaders with LinuxBoot

… on Saturday, 4:30 p. m. by Chris Koch

## What is LinuxBoot?

1.  Use Linux as boot-kernel and with initrd as bootloader
1.  Feasible due to increased sizes of flash ROM chips (thanks to UEFI firmware)
1.  Use defined interfaces (coreboot romstage, UEFI PEI, U-Boot SPL)

## Initrd

1.  Heads
1.  u-root
1.  Petitboot (Buildroot)
1.  Everything that fits
    1.  OpenEmbedded/Yocto

## Demo time

1.  ASRock E350M1
1.  AMD Fusion (APU, integrated graphics device)
1.  socketed 4 MB flash ROM chip by default
1.  4 GB RAM

# Booting fast

## Motivation

1.  Systems get faster, but still take some time
1.  Differentiate between firmware and OS
1.  Suspend to RAM is bad, and just a workaround (and adds unneded complexity)
1.  If you want to keep the state, use suspend to disk.
1.  How many power plants could be shut down?
1.  Customers should request fast boot times.
1.  Chromebooks and -boxes have boot time requirements.
1.  Even on servers, so you can just reboot with a downtime less than the TCP time-out.

## Past efforts

1.  [LPC: Booting Linux in five seconds](https://lwn.net/Articles/299483/)
1.  Ten years ago: September 2008
1.  Eee PC

## Platform

1.  Sever year old ASRock E350M1
1.  AMD Fusion
1.  Kingston SSD

## Firmware

1.  Use coreboot
1.  1 second with loading GRUB payload
1.  Option ROM and AGESA integration slow
1.  Siemens MB TCU3: Total Time: 377,319 (`siemens/mc_tcu3/4.4-108-g0d4e124/2016-05-09T06_14_45Z`)

## Operating system

1.  Linux kernel
2.  Initrd/initramfs
3.  User space

## Linux kernel

1.  Built it yourself
1.  Use `initcall_debug`
1.  kprobes
1.  systemd-bootchart
1.  `bootgraph.py` (with ftrace)
1.  Doesn’t seem much focus

## Initramfs

1.  Use LZ4 with SSD

         [    0.484102] calling  populate_rootfs+0x0/0x10f @ 1
         [    0.484127] Unpacking initramfs...
         [    0.538943] Freeing initrd memory: 29020K
         [    0.538955] initcall populate_rootfs+0x0/0x10f returned 0 after 53563 usecs

1.  Get rid of initramfs (most systems static)

## User space

1.  systemd-analyze
1.  systemd-bootchart
1.  perf
1.  Deactivate services
1.  Reorder services depending on system
1.  systemd-journal flush takes long
1.  udev rules

## ACPI S3

1.  `sleepgraph.py`

## What is needed?

1.  Interfaces to avoid reinitializing devices
1.  Pressure on device manufactures to care about boot time (NVMe, …)
1.  Different target types for desktops, servers, …
