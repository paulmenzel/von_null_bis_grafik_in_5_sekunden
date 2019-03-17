% Von Null bis Grafik in 5 Sekunden
% Paul Menzel
% 17. März 2019

## Wer bin ich?

- Systemarchitekt beim [Max-Planck-Institut für molekulare Genetik](https://www.molgen.mpg.de/)
- Diplom-Wirtschaftsmathematiker ([TU Berlin](https://www.tu-berlin.de/))
- FLOSS-Befürworter
- Seit 2005 im Projekt [coreboot](https://www.coreboot.org/) aktiv (damals LinuxBIOS)
- 2 Artikel im Linux Magazin

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

1.  Gegründet von Ron Minnich als LinuxBIOS beim [LANL](https://www.lanl.gov/)
1.  [„Press F1 to continue.“](http://www.h-online.com/open/features/The-Open-Source-BIOS-is-Ten-An-interview-with-the-coreboot-developers-746525.html)
1.  [The Linux BIOS](https://www.coreboot.org/images/1/14/Linuxbios.ps)
1.  [https://www.coreboot.org/Clusters](SC 2000: The first LinuxBIOS cluster, built at SC 2000, now at LANL)
1.  [1024-node linuxbios cluster with Dual-P4 systems and Myrinet](https://mail.coreboot.org/pipermail/coreboot/2002-September/000297.html)

# Schneller Start

## Motivation

1.  Trotz immer leistungsfähigeren Geräten dauert der Start immer noch lange.
1.  Unterschied durch SSD
1.  Wenige Hersteller fokussieren sich darauf. Google Chromebooks positive Ausnahme mit Bedingungen in weniger als 10 Sekunden zu starten.
1.  Unklar, warum von Kunden akzeptiert. (auch bei anderen Komponenten: Fernseher, Telefone)

## Einschub Ruhemodus (ACPI S3, neue Idle-Zustände S0ix)

1.  Bereitschaft (ACPI S3, Suspend to RAM) schlechte Lösung zum Lösung des Problems des langsamen Starts
1.  Wie viel Ressourcen in Entwicklung, Fehlerbehebung (Firmware)
1.  Suspend to Disk Lösung zum Speichern des Zustandes
1.  Wie viel Kraftwerke könnten weltweit gespart werden?

## Motivation

1.  Auch auf Servern könnten mit schnellen Startzeiten Wartung vereinfachen, wenn Neustart schneller ist als Standard-TCP-Zeitüberschreitung.

## Ähnliche Vorhaben in Vergangenheit

1.  [LPC: Booting Linux in five seconds](https://lwn.net/Articles/299483/)
1.  Von vor 10 Jahren: September 2008
1.  Eee PC

# Demo

## Demo

1.  sieben Jahre altes ASRock E350M1
1.  AMD Fusion (APU, integriertes Grafikgerät)
1.  ausgeliefert mit steckbarer 4-MB-Flash-ROM-Chip (Löschen von Block bis 400 ms)
1.  4 GB RAM
1.  Kingston SSD

## Demo: Ergebnis

1.  gut sechs Sekunden
1.  Firmware: coreboot mit GRUB-Payload: 1,5 Sekunden
1.  selbstgebautes Linux von master-Zweig ohne initrd: 1,1 Sekunden
1.  Debian mit systemd 241 und Weston 5.0.0 (Wayland): 3 bis 4 Sekunden

## Komponenten

1.  Firmware
1.  Betriebssystem
    1.  Linux-Kernel
    1.  Initrd/Initramfs
    1.  Userspace

## Firmware

1.  Standardmäßig BIOS/UEFI und alleine über 5 s – meist 8 bis 10 s
1.  Lösung: coreboot-basierende Firmware
1.  1 Sekunde mit GRUB-Payload (minimiertes `default_payload.elf`)

    1.  `make default_payloat.elf FS_PAYLOAD_MODULES="" EXTRA_PAYLOAD_MODULES="boottime"`
    1.  Komprimiert 177 KB in CBFS

1.  Option ROM and AGESA integration slow
1.  Siemens MB TCU3 with coreboot and SeaBIOS payload

        Total Time: 377,319

    aus *Board Status* `siemens/mc_tcu3/4.4-108-g0d4e124/2016-05-09T06_14_45Z`

## Firmware: LinuxBoot

1.  Linux as Payload: 416 KB mit `make tinyconfig`
1.  größer als GRUB-Payload und keine Shell
1.  Trotzdem Vorteile

## Operating system

1.  Linux kernel
2.  Initrd/initramfs
3.  User space

## Linux kernel

1.  Selbstbauen mit minimaler Konfiguration
1.  `initcall_debug` zeigt, wie lange Modul-Init-Routinen brauchen
1.  Kprobes
1.  systemd-bootchart bereitet Daten auf
1.  `bootgraph.py` (with ftrace)
1.  Schnelles Starten kein Schwerpunkt bei Entwicklerinnen und Entwicklern

## initcall\_debug

Aus `init/main.c`:

    static __init_or_module void
    trace_initcall_finish_cb(void *data, initcall_t fn, int ret)
    {
            ktime_t *calltime = (ktime_t *)data;
            ktime_t delta, rettime;
            unsigned long long duration;

            rettime = ktime_get();
            delta = ktime_sub(rettime, *calltime);
            duration = (unsigned long long) ktime_to_ns(delta) >> 10;
            printk(KERN_DEBUG "initcall %pF returned %d after %lld usecs\n",
                     fn, ret, duration);
    }

## Initrd/Initramfs

1.  Nutze LZ4 zum Komprimieren bei SSD

         [    0.484102] calling  populate_rootfs+0x0/0x10f @ 1
         [    0.484127] Unpacking initramfs...
         [    0.538943] Freeing initrd memory: 29020K
         [    0.538955] initcall populate_rootfs+0x0/0x10f returned 0 after 53563 usecs

1.  Verkleinern durch Einfügen von nur benötigten Modulen (statisches System)

    `MODULES=dep` in `/etc/initramfs-tools/initramfs.conf`

1.  Kein initramfs (schlecht bei Verschlüsselung)

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
