# Introduction to Linux Shell and Shell Scripting

# 1.1

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

# 1.2

## The Boot Process Flow

You need to know the chronological order of a Linux boot. If a system fails to start, knowing which stage it's stuck in tells you exactly what is broken.

* BIOS/UEFI: Performs the Power-On Self-Test (POST) and locates the bootloader.
* Bootloader (GRUB2): Loads the kernel and the initramfs into memory.
* Kernel: Initializes hardware and mounts the root filesystem (read-only initially).
* Init Process: The kernel starts the first process (PID 1), which is almost always systemd on modern distros.
* Targets/Runlevels: systemd brings up services (network, GUI, etc.) based on the default target.

## GRUB2 (Grand Unified Bootloader)
GRUB2 is the industry standard bootloader. You need to know how to navigate and configure it.

* Configuration Files: * /etc/default/grub: Where you make your manual edits (e.g., changing the timeout or adding kernel parameters).

      /boot/grub/grub.cfg: The main config file (Do not edit this manually; it is auto-generated).

* Applying Changes: After editing /etc/default/grub, you must run:

      update-grub (Debian/Ubuntu)

      grub2-mkconfig -o /boot/grub2/grub.cfg (RHEL/Fedora)

Interactive Menu: Pressing e at the GRUB menu allows you to edit boot parameters temporarily—useful for booting into "single-user mode" to reset a lost root password.

### Essential systemctl Commands:

     systemctl get-default: See if your system boots to CLI or GUI by default.
     systemctl set-default graphical.target: Change the default boot mode.
     systemctl isolate multi-user.target: Switch to a different mode immediately without rebooting.

## Initial RAM Filesystem (initramfs)

The kernel is often too small to contain every driver for every possible hard drive or RAID controller.

* The Job: initramfs is a small filesystem loaded into RAM that contains the specific drivers needed to mount the "real" root partition on the hard drive.
* Command: dracut or mkinitramfs are used to regenerate this file if you change hardware or update drivers.

## Shutdown and Reboot
While systemctl reboot is the modern way, the legacy shutdown command is still heavily tested.

    shutdown -h +10 "Maintenance starting": Halts the system in 10 minutes and sends a message to all logged-in users.

    shutdown -c: Cancels a pending shutdown.

# 1.3 

## Partitioning and Filesystems

You are expected to know how to prepare a raw disk for data.

* Partitioning Tables: Understand the difference between MBR (legacy, 4 primary partitions, 2TB limit) and GPT (modern, 128 partitions, massive capacity).

Tools:

            fdisk: Standard for MBR and basic GPT.

            gdisk: Specifically for GPT partitions.

            parted: A powerful tool that supports resizing and scriptable partitioning.

* Filesystem Types: Know when to use Ext4 (general purpose), XFS (high performance/large files), Btrfs (snapshots/pooling), and VfAT/ExFAT (cross-platform compatibility).

### Logical Volume Management (LVM)

This is a critical "bread and butter" skill for Linux admins. LVM allows you to resize partitions without unmounting them (in some cases) and aggregate multiple disks.

The Hierarchy:

            Physical Volumes (PV): Initializing a partition for LVM (pvcreate).

            Volume Groups (VG): Pooling PVs together (vgcreate).

            Logical Volumes (LV): Creating the "virtual" partitions users actually see (lvcreate).

* Key Operations: Extending a volume (lvextend), reducing a volume (lvreduce), and taking snapshots for backups.

## Mounting and Persistence

The kernel needs to know where to "hook" the storage into the directory tree.

* Manual Mounting: Using mount and umount.

* Persistent Mounting: Managing the /etc/fstab file. You must know the syntax:

            [Device/UUID] [Mount Point] [Filesystem Type] [Options] [Dump] [Pass]

* UUIDs: Why using the Universally Unique Identifier (blkid) is safer than using device names like /dev/sdb1 (which can change between reboots).

## Network Storage (NFS and SMB)

In a modern environment, storage isn't always local.

* NFS (Network File System): Standard for Linux-to-Linux sharing. Know the /etc/exports file and the showmount command.
* SMB/CIFS: Standard for Windows/Linux interoperability. Focus on using mount -t cifs and providing credentials securely.
