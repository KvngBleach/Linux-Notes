# Introduction to Linux Shell and Shell Scripting

## The Basic Boot Process
You are expected to understand the sequence of events from power-on to the login prompt.

* Bootloader: The software that starts the OS (e.g., GRUB2, LILO, Syslinux).
* Configuration: You must know where the config files live (e.g., /etc/default/grub and /boot/grub/grub.cfg).
* Kernel: The heart of the OS. You need to know how to pass Kernel Parameters at boot (like quiet, nomodeset, or root=).
* Initial RAM Disk (initrd/initramfs): A temporary file system that loads the drivers necessary to mount the real root file system.
* PXE (Preboot Execution Environment): Understanding how systems boot over a network, common in data centers and "diskless" setups.

## Filesystem Hierarchy Standard (FHS)

Directory	Purpose
/bin & /sbin	Essential binaries (commands) and system binaries for the root user.
/etc	Configuration files for the system and applications.
/proc & /sys	Virtual file systems containing Kernel and process information.
/var	Variable data: Logs (/var/log), mail, and spool files.
/usr	User programs, libraries, and documentation.
/boot	Files needed to start the boot process (Kernel images, GRUB).

## Server Architectures

Modern Linux admins deal with more than just standard PCs. You must recognize these architectures:

* x86_64 / AMD64: The standard 64-bit desktop and server architecture.
* AArch64 (ARM64): Common in cloud instances (like AWS Graviton) and Raspberry Pis.
* RISC-V: The emerging open-source instruction set architecture.

## Distributions and Licensing

* Distribution Families: Distinguishing between RPM-based (RHEL, Fedora, Rocky) and Debian-based (Ubuntu, Kali).
* Software Licensing: Understanding the basics of Open Source (GPL, MIT, Apache) vs. proprietary software.
