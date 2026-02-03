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

#3 Storage Health and Troubleshooting

You need to be able to detect when a disk is failing or full.

* Monitoring Space: df -h (disk free) and du -sh (disk usage).
* Health Checks: smartctl (S.M.A.R.T. monitoring) to check for hardware failure indicators.
* Inodes: Understanding that a disk can be "full" even if it has gigabytes of space left if it runs out of Inodes (trackers for small files). Use df -i to check this.

Task,Command
 View disk partitions,      lsblk or fdisk -l
 Create a filesystem,      mkfs.ext4 /dev/sdb1
 Check filesystem integrity,      fsck /dev/sdb1
List Volume Groups,      vgs or vgdisplay
Get Device UUID,            blkid
Mount all in fstab,         mount -a

# 1.4

## Interface Configuration
You need to know how to identify and configure network interfaces.

* Naming Conventions: Understand why interfaces aren't just eth0 anymore. Modern systems use Predictable Network Interface Names (like enp0s3), which are based on physical hardware locations.

* Commands

            ip addr: The modern replacement for ifconfig. Used to view and assign IP addresses.

            ip link: Used to activate or deactivate an interface (e.g., ip link set dev eth0 up).

            nmcli: The command-line interface for NetworkManager. This is the primary tool for RHEL/Fedora/Ubuntu desktop systems.

            nmtui: The text-based user interface (great for exam scenarios where you need a "GUI-like" feel in a terminal).

## Name Resolution (DNS)

If you can ping an IP but not a website, it’s a DNS issue.

            /etc/hosts: The first place the system looks for local name-to-IP mappings.

            /etc/resolv.conf: Contains the IP addresses of the DNS servers your system queries.

                  Note: On modern systems, this file is often managed by systemd-resolved.

            /etc/nsswitch.conf: Determines the order in which the system looks for names (e.g., "Check /etc/hosts first, then check DNS").

            Tools: dig and host are the standard tools for testing DNS records.

## Routing and Gateways

Getting traffic out of your local network and onto the internet.

            The Routing Table: View it using ip route or route -n.

            Default Gateway: The "exit door" for any traffic not destined for the local subnet. You can add one using ip route add default via [IP].

            IP Forwarding: Crucial for Linux machines acting as routers. This is toggled in /proc/sys/net/ipv4/ip_forward.

## Troubleshooting Tools

The exam will likely give you a "broken" scenario and ask which tool to use.

ping	Checks basic connectivity (ICMP).
traceroute - Shows every "hop" (router) between you and the destination.
mtr -	A "live" version of traceroute that combines ping and path tracing.
ss -	(Socket Statistics) Replaces netstat. Shows open ports and active connections.
tcpdump -	A packet sniffer. Used for deep-dive analysis of what is actually hitting the wire.
nslookup - 	A legacy tool for DNS queries (prefer dig for the exam).

# 1.5

## Localization (Locales) 

Locales define the language, country, and specific character encoding (like UTF-8) for the system.

* The Concept: A locale string looks like en_US.UTF-8.

      en = Language (English)

      US = Territory (United States)

      UTF-8 = Codeset (Character encoding)

* Key Commands:

      localectl: The modern systemd tool to view and set the system locale and keyboard layout.

      locale: Displays the current environment variables (like LANG, LC_TIME, and LC_ALL).

      locale -a: Lists all currently available locales on the system.

## Time and Date Management

In a networked world, having the wrong time can break SSL/TLS certificates and make log analysis impossible.

System Time vs. Hardware Clock:

            System Time: Maintained by the Linux kernel.

            Hardware Clock (RTC): The battery-backed clock on the motherboard.

* Commands:

      date: Views or sets the system time.

      hwclock: Synchronizes the system clock and the hardware clock (--systohc or --hctosys).

      timedatectl: The primary systemd command for managing time, time zones, and NTP status.

      Time Zones: Managed via /etc/localtime (usually a symbolic link to a file in /usr/share/zoneinfo/).

## Network Time Protocol (NTP)

Keeping clocks in sync across multiple servers.

* Chrony: The modern default NTP client/server for many distros (RHEL, Fedora). It’s designed to work well even on systems that are frequently offline or have unstable connections.

        Config file:/etc/chrony.conf

* systemd-timesyncd: A lightweight NTP client built into systemd, used for simple SNTP (Simple Network Time Protocol) syncing.
* ntp (ntpd): The classic, legacy NTP daemon.

## User and Global Profiles

File-Scope-Description
/etc/profile - Global - Set for all users upon login.
/etc/bashrc (or bash.bashrc) - Global - Functions and aliases for all users in Bash.
~/.bash_profile - User - User-specific environment settings (runs once at login).
~/.bashrc,- User - User-specific aliases and functions (runs for every new terminal).

## Environment Variables

These are dynamic "values" that the system and applications reference.

* PATH: Arguably the most important variable. It defines the directories where the system looks for executable commands.
* EDITOR: Defines the default text editor (e.g., vi or nano).
* HOME: The current user's home directory.
* Commands: * export VARNAME=value: Sets a variable for the current session and sub-processes.

        env: Lists exported environment variables.

Feature,timedatectl,localectl
Change Timezone- set-timezone[Zone]- N/A
Change Language- N/A- set-locale LANG=[Locale]
Enable NTP- set-ntp true- N/A
Change Keyboard- N/A- set-x11-keymap [layout]

# 1.6

## Package Management (The Big Three)

Linux doesn't use "InstallShield" wizards. It uses package managers. You need to know the specific commands for the two major families and the emerging universal formats.

### The Debian/Ubuntu Family (DEB)

* Low-level tool: dpkg (Used for local .deb files; doesn't handle dependencies).
* High-level tool: apt (The modern standard) or apt-get (legacy/scripting).
* Key commands: * apt update: Refreshes the list of available software.

      apt upgrade: Installs the latest versions of all packages.

      apt search [app]: Finds a package in the repositories.

### The RHEL/Fedora/CentOS Family (RPM)

* Low-level tool: rpm (Used for local .rpm files).
* High-level tool: dnf (The modern standard, replaces the older yum).
* Key commands:

      dnf install [package]: Installs with dependency resolution.

      dnf provides [file]: A "magic" command that tells you which package contains a specific missing file.

### Universal/Sandboxed Packages

These work on almost any distro and bundle all their dependencies together.

* Snap: Developed by Canonical (Ubuntu). Managed via the snap command.
* Flatpak: Popular for desktop Linux. Managed via the flatpak command.
* AppImage: A single executable file; no installation required.

## Managing Services with systemd

Most modern distros use systemd to manage background services (daemons). The systemctl command is your primary tool here.

Task,Command
* Start a service immediately- systemctl start [service]
* Stop a service immediately- systemctl stop [service]
* Make a service start at boot- systemctl enable [service]
* Prevent a service from starting at boot- systemctl disable [service]
* Check if a service is running/failed- systemctl status [service]
* Restart and apply config changes- systemctl restart [service]
* Completely block a service from starting- systemctl mask [service]

## Compiling from Source

Sometimes software isn't in a repository, and you have to build it yourself. You must know the "Holy Trinity" of commands:

      ./configure: Checks your system for required libraries and creates a Makefile.

      make: Compiles the source code into binary files.

      sudo make install: Moves those binaries into the system directories (like /usr/local/bin).

## Shared Libraries

Software often relies on shared code libraries (files ending in .so).

* ldd: Shows which libraries a specific program needs to run.
* ldconfig: Updates the library cache so the system can find newly installed libraries.
* /etc/ld.so.conf: The configuration file that lists directories where the system looks for libraries.

## Software Repositories

You need to know where the "list of stores" is kept.

* Debian/Ubuntu: Managed in /etc/apt/sources.list and /etc/apt/sources.list.d/.
* RHEL/Fedora: Managed in /etc/yum.repos.d/.

### Critical Exam Scenario: "The Missing Dependency"
If the exam asks how to fix a broken installation:

      On Ubuntu, you’d use apt --fix-broken install.

      On RHEL, you’d use dnf check or dnf history rollback to undo a botched update.

## Automation and Orchestration Concepts

This is the theoretical heart of the section. You need to distinguish between automation (one task) and orchestration (a workflow of many automated tasks).

* Infrastructure as Code (IaC): Managing and provisioning infrastructure through machine-readable definition files rather than physical hardware configuration or interactive configuration tools.
* Idempotency: A core concept in tools like Ansible. It means that no matter how many times you run a script, the result remains the same without causing errors or duplicate configurations.
* Self-healing: Systems that automatically detect a failure (like a downed service) and restart or replace it without human intervention.
* Agent vs. Agentless: * Agent-based: Requires software installed on the target (e.g., Puppet, Chef).
* Agentless: Operates over standard protocols like SSH (e.g., Ansible).

## Verison Control (Git)

You are expected to understand the basic lifecycle of code management, as scripts and configuration files are now treated like software.

* Core Commands: git init, add, commit, push, pull, clone, and merge.
* Branching: Understanding how to work on a "feature branch" before merging it into the "main/master" branch.
* Repositories: The difference between local repositories and remote ones (GitHub, GitLab, Bitbucket).

## Orchestration and Configuration Tools

While you don't need to be a developer, you must recognize the syntax and use cases for:

* Ansible: Uses YAML files called Playbooks. It is the most common tool featured on the exam due to its simplicity and Linux-native feel.
* Terraform: Uses HCL (HashiCorp Configuration Language). It is used for "provisioning" (creating the server itself) rather than "configuring" (installing apps on the server).
* Kubernetes (K8s): Basic awareness of container orchestration—how it manages the deployment and scaling of containers.

## Scheduling Tasks

This is the "old school" automation that is still vital today.

* Cron: Familiarize yourself with the crontab format:

            * * * * * command_to_execute (Minute, Hour, Day of Month, Month, Day of Week).

* Systemd Timers: The modern Linux replacement for Cron. You should know that they offer better logging via journalctl and can be more precise than Cron.

* at: Used for one-time future tasks (e.g., at 10:00 PM).


5. Scripting Best Practices
