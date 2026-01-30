# Introduction to Linux Shell and Shell Scripting

## Distributions and Lifecycles

You need to distinguish between the various "flavors" of Linux and how they are maintained.

* Distribution Families: Understanding the differences between Debian-based (Ubuntu, Kali), RHEL-based (Fedora, AlmaLinux, Rocky), and SUSE-based systems.
* Software Repositories: Knowing the role of stable vs. testing/unstable repos.
* Maintenance Cycles: * LTS (Long-Term Support): Focused on enterprise stability (usually 5–10 years of updates).
* Rolling Release: Focused on the "bleeding edge" (updates are constant, no major version jumps).
* End of Life (EOL): Managing the risks when a version no longer receives security patches.

## Kernel Components and Modules

The kernel is the heart of the OS. You must understand how it interacts with hardware and how to tweak it without a full reboot.

* Kernel Roles: Managing memory, CPU scheduling, and hardware abstraction.
* Kernel Modules: Using tools like lsmod, insmod, modprobe, and rmmod to add or remove drivers on the fly.
* Parameters: Understanding how /proc/sys/ and sysctl allow you to modify kernel behavior at runtime.

## The Boot Process

You are expected to know exactly what happens from the moment you hit the power button until you see a login prompt.

* Hardware Initialization: BIOS vs. UEFI.
* Bootloaders: Primarily GRUB2. You should know how to navigate the GRUB menu and pass temporary parameters to the kernel (like booting into single-user mode).
* Init Systems: While systemd is the modern standard (Targets, Units, and Services), you still need a passing familiarity with the legacy SysVinit (Runlevels).
* The Initial RAM Disk: The role of initramfs or initrd in loading necessary drivers to mount the root file system.

# Comparison Table: Common Distro Families

Feature	Debian / Ubuntu	RHEL / Fedora / Rocky
Package Manager -	apt / dpkg	dnf / rpm
Package Format -	.deb	.rpm
Init System	- systemd	systemd
Enterprise Focus	General Purpose / Cloud	Corporate / High-Security

## Breakdown of Domain 1.2: System Access and Interaction

1. Remote Access Protocols and Tools

* SSH (Secure Shell): The gold standard. You need to understand:
     Public Key Authentication: Using ssh-keygen, ssh-copy-id, and the authorized_keys file.
     Port Forwarding/Tunneling: Securely routing traffic through an SSH connection.
* Console Access: Understanding TTY (Teletype) and PTS (Pseudo Terminal Slave) sessions.
* VNC and RDP: Methods for remote graphical access (though less common in pure server administration).

2. The Command Line Interface (CLI)

You are expected to understand the environment in which you type your commands.

* Shell Types: Primarily Bash (the default), but also awareness of Zsh (popular in macOS/modern distros) and Fish.
* Shell Configuration: Knowing which files load when you log in:

/etc/profile (Global)

~/.bash_profile or ~/.bashrc (User-specific)

* Environment Variables: Using export, env, and understanding $PATH (where the system looks for executable programs).

