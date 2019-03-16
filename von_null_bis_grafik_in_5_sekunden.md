% Von Null bis Grafik in 5 Sekunden
% Paul Menzel
% 17. März 2019

## Wer bin ich?

- Systemarchitekt beim [Max-Planck-Institut für molekulare Genetik](https://www.molgen.mpg.de/)
- Diplom-Wirtschaftsmathematiker ([TU Berlin](https://www.tu-berlin.de/))
- FLOSS-Befürworter
- Seit 2005 im Projekt [coreboot](https://www.coreboot.org/) aktiv (damals LinuxBIOS)

## Präsentation/Folien

Mit Markdown und Pandoc erstellt, Quellen verfügbar.

TinyURL: <https://tinyurl.com/vonnullbisgrafikinfuenfsekunden>

<https://github.com/paulmenzel/von_null_bis_grafik_in_5_sekunden>

# Einführung

## Warum?

1.  Warnung: Fokus auf x86
1.  Ziel: Schnell starten
1.  Interessantes und spannendes Thema ohne Ende
1.  Tolle Gemeinschaft aus unterschiedlichen Bereichen

## Geschichte von coreboot/LinuxBIOS

1.  Started by Ron Minnich as LinuxBIOS at [LANL](https://www.lanl.gov/)
1.  [„Press F1 to continue.“](http://www.h-online.com/open/features/The-Open-Source-BIOS-is-Ten-An-interview-with-the-coreboot-developers-746525.html)
1.  [The Linux BIOS](https://www.coreboot.org/images/1/14/Linuxbios.ps)
1.  [https://www.coreboot.org/Clusters](SC 2000: The first LinuxBIOS cluster, built at SC 2000, now at LANL)
1.  [1024-node linuxbios cluster with Dual-P4 systems and Myrinet](https://mail.coreboot.org/pipermail/coreboot/2002-September/000297.html)

## Demo

1.  ASRock E350M1
1.  AMD Fusion (APU, integriertes Grafikgerät)
1.  ausgeliefert mit steckbarer 4-MB-Flash-ROM-Chip (Löschen von Block bis 400 ms)
1.  4 GB RAM

# Schneller Start

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

## Demo platform

1.  Seven year old ASRock E350M1
1.  AMD Fusion
1.  Kingston SSD

## Firmware

1.  Use coreboot
1.  1 second with loading GRUB payload (minimized `default_payload.elf`)
1.  Option ROM and AGESA integration slow
1.  Siemens MB TCU3 with coreboot and SeaBIOS payload: Total Time: 377,319 (`siemens/mc_tcu3/4.4-108-g0d4e124/2016-05-09T06_14_45Z`)

## Operating system

1.  Linux kernel
2.  Initrd/initramfs
3.  User space

## Linux kernel

1.  Build it yourself
1.  Use `initcall_debug`
1.  kprobes
1.  systemd-bootchart
1.  `bootgraph.py` (with ftrace)
1.  Doesn’t seem much focus

## Initrd/Initramfs

1.  Use LZ4 with SSD

         [    0.484102] calling  populate_rootfs+0x0/0x10f @ 1
         [    0.484127] Unpacking initramfs...
         [    0.538943] Freeing initrd memory: 29020K
         [    0.538955] initcall populate_rootfs+0x0/0x10f returned 0 after 53563 usecs

1.  Make it smaller by only using necessary modules

    `MODULES=dep` in `/etc/initramfs-tools/initramfs.conf`

1.  Get rid of initramfs (most systems static)

## User space

1.  systemd-analyze
1.  systemd-bootchart
1.  strace – trace system calls and signals
1.  perf – Performance analysis tools for Linux
1.  Deactivate services
    1.  for example ModemManager not needed on desktop systems
1.  Reorder services depending on system
1.  systemd-journal flush takes long
1.  udev rules

## ACPI S3

1.  `sleepgraph.py`

# What to do?

## Users

1.  Support vendors caring about these things.
1.  Use Power
    1.  OpenPower Foundation
    1.  Workstations available: https://www.raptorcs.com/TALOSII/
1.  Resellers for older devices
1.  Purism devices
1.  Reseller for used Facebook Open Compute Project systems: http://www.horizon-computing.com/
1.  Google Chromebooks and -boxes (Intel and ARM)
1.  Dell: Systems with GNU/Linux preinstalled, has Linux developers, and LVFS support for a long time

## What is needed to improve the situation?

1.  Interfaces to avoid reinitializing devices
1.  Pressure on device manufactures to care about boot time (NVMe, …)
1.  Different target types for desktops, servers, …
1.  Focus on fast startup times
    1.  Integrate profiling tools in systemd
