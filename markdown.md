# Descript Breakdowns

## 1.1 Configure and Manage Software

This objective ensures you can handle the two major "families" of Linux. You must be able to install, update, and remove software without breaking dependencies.

* Package Managers: * Debian-based: apt, apt-get, and dpkg.
* RHEL-based: dnf (the focus over yum) and rpm.
* Zypper: Used in SUSE/openSUSE.
* Repositories: How to add new sources in /etc/apt/sources.list or /etc/yum.repos.d/.
* Shared Libraries: Understanding how programs find their code using ldd to view dependencies and ldconfig to update the cache.

## 1.2 Analyze System Boot Components
You need to know what happens from the moment you hit the power button until you see a login prompt.

The Boot Chain: BIOS/UEFI → Bootloader (GRUB2) → Kernel → Initramfs → systemd.

GRUB2: Managing /etc/default/grub and running grub-mkconfig. You should know how to temporarily edit a boot entry to enter "single-user mode" or rescue.target.

Initrd/Initramfs: The temporary file system that loads the drivers needed to mount the real root partition.

## 1.3 Configure and Manage Storage Units
This is a high-stakes area. If you mess up storage in a Performance-Based Question (PBQ), the system won't boot.

* Partitioning: Using fdisk (MBR) and gdisk (GPT).
* LVM (Logical Volume Management): You must know the hierarchy:
* PV (Physical Volume): pvcreate
* VG (Volume Group): vgcreate
* LV (Logical Volume): lvcreate

File Systems: Creating and maintaining ext4, XFS, and Btrfs. Know how to check them with fsck or xfs_repair.

Mounting: Persistent mounting via /etc/fstab. You must understand the fields (UUID, mount point, type, options).

## 1.4 Configure and Manage Network Settings
The exam has moved almost entirely to the iproute2 suite and NetworkManager.

* Manual Config: Using ip addr, ip link, and ip route.
* NetworkManager: Mastering nmcli (command line) and nmtui (text UI).
* Hostname/DNS: Configuring /etc/hostname, /etc/hosts, and /etc/resolv.conf.
* Troubleshooting Tools: dig and nslookup for DNS; ping, traceroute, and mtr for connectivity.

## 1.5 Configure and Manage Hardware and Kernel
Understanding the bridge between the software and the physical (or virtual) machine.

* Kernel Modules: Using lsmod (list), modprobe (smart insert/remove), and modinfo.
* Hardware Discovery: lsusb, lspci, and lscpu.
* Monitoring: Checking the "kernel ring buffer" via dmesg to find hardware errors or driver failures.

The "v8" Performance Focus
A common PBQ for Domain 1 involves LVM Expansion. They might give you a server running out of space and a new raw disk. Your workflow would be:

* pvcreate the new disk.
* vgextend the existing group.
* lvextend the logical volume.
* resize2fs (for ext4) or xfs_growfs (for XFS) to actually use the space.

### Step 1: Initialize Physical Volumes (PV)
Tell Linux to treat raw disks/partitions as LVM-ready.

      Command: pvcreate /dev/sdb1 /dev/sdc1

      Verification: pvdisplay or pvs

### Step 2: Create a Volume Group (VG)
Pool those physical disks into one big "bucket" of storage.

      Command: vgcreate vg_data /dev/sdb1 /dev/sdc1

      Verification: vgdisplay or vgs

### Step 3: Carve out a Logical Volume (LV)
This is what you actually format and use.

      Command: lvcreate -L 50G -n lv_projects vg_data

      -L: Size (e.g., 50 Gigabytes)

      -n: Name of the volume

      Verification: lvdisplay or lvs

### Step 4: Format and Mount
Now it behaves like a normal disk.

      Command: mkfs.ext4 /dev/vg_data/lv_projects

      Command: mount /dev/vg_data/lv_projects /projects

## 3. Storage Troubleshooting Commands

On the XK0-006, if a user says "I'm out of space," these are your go-to tools:

* df -h: Shows disk space (Human-readable).

* du -sh /folder: Shows how much space a specific directory is using.

* lsblk: Shows the "tree" view of your blocks (partitions, LVM, and mount points).

* blkid: Used to find the UUID of a disk (essential for fstab).

* fsck: Used to repair a corrupted filesystem (must be unmounted first!).

Quick Scenario Practice:
Question: A technician adds a new 1TB drive and wants it to mount automatically at boot to /backup using the XFS filesystem. They want the system to check it for errors after the root drive is checked.

What would the /etc/fstab line look like?

      UUID=xxxx-xxxx /backup xfs defaults 0 2

## Ansible Playbook Anatomy

---
      - name: Configure Apache Web Servers  # Description of the play
        hosts: web_servers                 # The group of servers from your inventory
        become: yes                        # Run as sudo/root

         tasks:
        - name: Install apache2 package  # Description of the specific task
      apt:                           # The module being used (Debian/Ubuntu)
        name: apache2
        state: present               # "present" means install it

    - name: Start the apache service
      service:                       # The module for managing services
        name: apache2
        state: started
        enabled: yes                 # Make sure it starts on boot


 File Path                      |Purpose

/etc/services,                  |Lists port numbers and their associated protocols (e.g., 80 = HTTP)."

/etc/sysconfig/network-scripts/ |(Legacy RHEL) Where interface config files live.

 /etc/netplan/                  |(Modern Ubuntu) Where YAML-based network configs live.

 /etc/protocols                 |Lists available internet protocols (TCP, UDP, ICMP)."

## 2.1 Manage Users and Groups
This is about maintaining the identity and security of who accesses the system.

Account Creation: Using useradd (low-level) and adduser (interactive), groupadd, and usermod.

The "Big Three" Files:

* /etc/passwd: User account info (UID, GID, Home Dir, Shell).
* /etc/shadow: Encrypted passwords and aging info.
* /etc/group: Group definitions.
* Password Management: Using passwd and chage to set expiration dates and minimum lengths.
* Profiles & Skeletons: Managing /etc/skel to give new users default files (like .bashrc).

## 2.2 Create and Modify Text Files
You must be efficient at editing configurations and processing data output.

* Editors: Proficiency in vi or vim is almost mandatory for the Performance-Based Questions (PBQs). Know how to save (:wq!) and exit (:q!).
* Output Processing: The "Pipeline" tools:
* grep (searching strings).
* awk and sed (stream editing and field manipulation).
* cut, sort, uniq, and wc (word count).
* Redirection: Mastering > (overwrite), >> (append), and 2> (redirect errors).

## 2.3 Configure and Manage System Services
This covers the core background processes that provide functionality to the network.

* Service Control: Deep dive into systemd.
* systemctl list-units --type=service.
* Managing targets (e.g., changing from multi-user.target to graphical.target).
* Common Network Services: You don't need to be a web dev, but you must know how to start/stop and locate logs for:
* Web: Apache (httpd) / Nginx.
* Time: chronyd or ntpd.
* SSH: sshd.
* Logging: Configuring rsyslog and using logger to test manual log entries.

## 2.4 Manage Container Operations
This is the most "modern" part of Domain 2. CompTIA treats containers as a core admin task now.

* Container Engines: Focus on Podman (RHEL-style) and Docker.

Essential Commands:

* pull: Fetching images from a registry.
* run: Starting a container (know flags like -d for detached and -p for port mapping).
* ps: Viewing running containers (-a for all).
* exec: Running a command inside a running container.
* logs: Troubleshooting a failing container.
* Container Images: Basic understanding of a Dockerfile (FROM, RUN, EXPOSE, CMD).

## 2.5 Configure and Manage Local and Network Environment Variables
Ensuring the system "knows" where things are.

* Environment Variables: PATH, HOME, USER, and SHELL.
* Persistent Variables: Editing ~/.bashrc, ~/.bash_profile, and /etc/profile to make changes stick.
* Locales: Using localectl to manage system language and keyboard layouts.

The "v8" Service Troubleshooting Workflow
A typical Domain 2 question might look like this:

"A user says the web server is down. You see the service is running, but the site won't load."

Your v8 Workflow:

* Check service status: systemctl status nginx.
* Check the port: ss -tulpn | grep 80.
* Check the logs: tail -f /var/log/nginx/error.log.
* Check the container (if applicable): podman logs [container_id].

## 3.1 Summarize authentication and accounting concepts.
This section moves beyond local users to how Linux fits into a larger corporate identity ecosystem.

* Authentication Frameworks: Understanding PAM (Pluggable Authentication Modules) is critical. You need to know how the /etc/pam.d/ directory works and the four management groups (Account, Authentication, Password, Session).
* Identity Management: Integration with LDAP, Active Directory (AD), and Kerberos.
* MFA (Multi-Factor Authentication): Knowledge of TOTP (Time-based One-Time Password) and hardware tokens.
* Auditing: Using auditd to track system changes and user activity.

## 3.2 Configure and maintain firewalls.
You are expected to know the "Big Three" of Linux firewalls. CompTIA tests your ability to translate a business requirement (e.g., "block all traffic except web") into specific commands.

* Firewalld: Managing Zones, services, and runtime vs. permanent configurations.
* UFW (Uncomplicated Firewall): Common in Ubuntu/Debian; simple syntax for allow/deny rules.
* nftables/iptables: Understanding the underlying framework (chains, tables, and rules) and how nft is replacing iptables.

## 3.3 Implement OS hardening techniques.
Hardening is about reducing the "attack surface." This is a high-yield area for Performance-Based Questions (PBQs).

* SSH Hardening: Disabling root login, enforcing key-based authentication, and changing default ports.
* Service Hardening: Disabling unused services and protocols (Telnet, FTP, etc.).
* Sudoers: Managing /etc/sudoers via visudo and applying the Principle of Least Privilege.
* Banner/MOTD: Setting up legal warning banners and Message of the Day.
* Shell Restrictions: Implementing restricted shells for specific users.

## 3.4 Implement Mandatory Access Control (MAC).
You don't need to be a developer, but you must know how to troubleshoot and manage the two main MAC systems.

* SELinux (Security-Enhanced Linux): * Modes: Enforcing, Permissive, Disabled.
* Tools: getenforce, setenforce, sestatus, and restorecon.
* Contexts: Identifying and fixing incorrect security contexts on files.
* AppArmor: Using aa-status, aa-complain, and aa-enforce.

## 3.5 Summarize cryptography and compliance concepts.
This is the "theory and standards" portion of the domain.

* Encryption at Rest: Understanding LUKS (Linux Unified Key Setup) and dm-crypt.
* Encryption in Transit: Managing SSL/TLS certificates and using OpenSSL for basic tasks (generating CSRs, etc.).
* Hashing: Using sha256sum or md5sum for file integrity checks.
* Vulnerability Management: Monitoring CVEs (Common Vulnerabilities and Exposures) and using scanners like OpenSCAP.

## 4.1 Create simple shell scripts to automate common tasks.
This is the core "classic" scripting section. You need to be able to read, write, and troubleshoot Bash scripts.

* Variables & Scope: Working with environment variables vs. local variables and positional parameters (e.g., $1, $2, $@).
* Control Statements: Using if/then/else, case statements, and loops (for, while, until).
* Logic & Comparison: Understanding exit codes ($?), logical operators (&&, ||), and bracket syntax for comparisons ([ ] vs [[ ]]).
* Text Processing: Using grep, sed, awk, cut, and sort within scripts to parse logs or configuration files.

## 4.2 Perform basic Python scripting.
New to V8. You aren't expected to be a full-stack developer, but you must understand Python's role in system automation.

* Python Fundamentals: Basic syntax, data types (strings, lists, dictionaries), and indentation rules.
* Execution: Running scripts with python3, using the shebang (#!/usr/bin/python3), and managing virtual environments (venv).
* Built-in Modules: Familiarity with modules like os, sys, and subprocess for interacting with the Linux system.
* Best Practices: Following PEP 8 style guidelines and basic error handling (try/except).

## 4.3 Perform basic version control using Git.
New to V8. The exam assumes you are storing your scripts and configuration files in a repository.

* Workflow: Understanding the lifecycle: git init → git add → git commit → git push.
* Branching: Creating and merging branches (git branch, git merge, git checkout).
* Collaboration: Cloning repositories (git clone) and pulling updates (git pull).
* Management: Using .gitignore to keep secrets/temp files out of the repo and using git log to view history.

## 4.4 Summarize orchestration and Infrastructure as Code (IaC) concepts.
This is primarily conceptual, though you should recognize the syntax of common tools.

* Configuration Management: Understanding the "push vs. pull" models and identifying tools like Ansible (YAML-based, agentless), Puppet, and Chef.
* Provisioning Tools: The role of Terraform or OpenTofu in managing cloud resources.
* CI/CD Pipelines: Understanding how automated testing and deployment work in a Linux environment.
* Orchestration: Basic concepts of Kubernetes and Docker Swarm for managing containerized applications.

## 4.5 Summarize AI-related best practices.
New to V8. This objective reflects the rise of AI in IT.

* Code Generation: Using AI to draft scripts while emphasizing the need for manual verification (never "blindly" copy-pasting).
* Prompt Engineering: How to write effective prompts to troubleshoot Linux errors or generate configuration templates.
* Security & Ethics: Awareness of leaking proprietary data or secrets (like API keys) into public AI models.

## 5.1 Analyze system properties and processes to troubleshoot issues.
This objective covers the "health check" phase. You need to know which tools reveal the source of a slowdown or crash.

* System Monitoring: Using top, htop, vmstat, and sar to identify resource hogs.
* Log Analysis: Proficiency with journalctl (systemd logs) and viewing traditional logs in /var/log/ (e.g., messages, secure, dmesg).
* Process Management: Identifying zombie processes, hanging tasks, and using kill, pkill, or nice/renice to manage priorities.
* Hardware Detection: Using lsusb, lspci, and lscpu to verify that the OS "sees" the hardware.

## 5.2 Troubleshoot user, service, and application issues.
This is the "break/fix" section for software and access.

* Permissions & Ownership: Diagnosing Permission Denied errors. You must know how to trace these to chmod, chown, or umask settings.
* Service Failures: Using systemctl status to find why a service failed to start and checking dependency loops.
* Authentication: Troubleshooting SSH keys, sudo access (the /etc/sudoers file), and account lockouts.
* Library Dependencies: Using ldd to find missing shared libraries that prevent an application from launching.

## 5.3 Troubleshoot storage and boot issues.
This section is critical because a storage failure often means a system won't boot at all.

* Boot Failures: Diagnosing GRUB errors, "Kernel Panics," and problems with the Initramfs.
* Drive Health: Using smartctl for hardware health and fsck to repair corrupted filesystems.
* Mounting Issues: Troubleshooting the /etc/fstab file (e.g., a system failing to boot because a non-critical drive is missing).
* Capacity Issues: Detecting "Disk Full" errors using df -h and du, and specifically checking for inode exhaustion (df -i).

## 5.4 Troubleshoot network and security issues.
Networking is often the most complex part of troubleshooting due to the layers involved.

* Connectivity: Moving up the stack from ping (ICMP) to traceroute (routing) to dig/nslookup (DNS).
* Port & Interface Issues: Using ss or netstat to see if a service is actually listening on a port, and ip addr to check interface states.
* Firewall Denials: Identifying if iptables, nftables, or firewalld is silently dropping traffic.
* Security Contexts: SELinux and AppArmor are high-priority. You must know how to check if a "denial" is caused by a security policy (e.g., getenforce, sestatus).

## 5.5 Summarize performance optimization concepts.
This is about proactive tuning rather than reactive fixing.

* Kernel Tuning: Understanding /proc/sys and the sysctl command to modify kernel parameters on the fly.
* Storage Optimization: Identifying I/O wait times and choosing the right scheduler or swap space configuration.
* Network Tuning: Adjusting MTU sizes and buffer settings for high-latency or high-bandwidth environments.

The Troubleshooting Methodology
CompTIA expects you to follow their specific 6-step troubleshooting process in scenario questions:

* Identify the problem (Ask the user, check logs).
* Establish a theory of probable cause.
* Test the theory to determine the cause.
* Establish a plan of action and implement the solution.
* Verify full system functionality (and implement preventive measures).
* Document findings, actions, and outcomes.

# Domain 1 Keywords

* BIOS/UEFI: The firmware interfaces that initialize hardware. UEFI is the modern standard, supporting larger disks and Secure Boot.
* GRUB2 (Grand Unified Bootloader): The primary bootloader for most Linux distros. It allows you to select kernels or edit boot parameters (like entering single-user mode).
* initramfs (Initial RAM Filesystem): A temporary file system loaded into RAM that contains the drivers and tools needed to mount the real root file system.
* systemd: The modern init system and service manager (PID 1). It uses units (like .service or .target) to manage the system state.
* Targets: systemd’s version of runlevels (e.g., multi-user.target for command line, graphical.target for desktop).
* Kernel Module: A piece of code (often a driver) that can be loaded into or unloaded from the kernel on the fly using insmod, rmmod, or modprobe.
* MBR vs. GPT: Partitioning schemes. MBR is legacy (4 primary partitions, 2TB limit); GPT is modern (128 partitions, virtually no size limit).
* LVM (Logical Volume Management): A layer of abstraction over physical disks.
* PV (Physical Volume): The actual disk or partition.
* VG (Volume Group): A pool created from one or more PVs.
* LV (Logical Volume): The "virtual partition" you format and mount.
* RAID (Redundant Array of Independent Disks): * RAID 0: Striping (Performance, no redundancy).
* RAID 1: Mirroring (Redundancy).
* RAID 5/6: Striping with Parity (Balance of speed and safety).
* VFS (Virtual File System): The kernel layer that allows Linux to support many different filesystem types (Ext4, XFS, Btrfs) using the same commands.
* Swap: A dedicated area on disk used as "overflow" RAM when physical memory is full.
* NetworkManager: The daemon that manages network connections. Controlled via nmcli (command line) or nmtui (text UI).
* iproute2: The modern collection of networking utilities (the ip command) that replaces the old ifconfig.
* DNS (Domain Name System): Resolves hostnames to IPs. Key files include /etc/resolv.conf (nameservers) and /etc/hosts (local overrides).
* DHCP: Automatically assigns IP addresses.
* SSH (Secure Shell): The standard for encrypted remote management.
* Repository: A central server containing software packages and metadata.
* DNF/YUM: Package managers for RHEL/Fedora-based systems.
* APT: Package manager for Debian/Ubuntu-based systems.
* Shared Libraries: Files (ending in .so) that contain code multiple programs can use simultaneously. Managed/linked via ldconfig.
* lsblk: List block devices (disks).
* df -h: Show human-readable disk space usage.
* dmesg: View kernel ring buffer messages (great for hardware troubleshooting).
* systemctl: The Swiss Army knife for managing systemd services.
* modprobe: The smart way to add/remove kernel modules (handles dependencies).
# Doamin 2 Keywords

UID / GID: User ID and Group ID. Linux identifies users and groups by these numbers, not their names. Root is always 0.

/etc/passwd: The text file containing user account information (username, UID, GID, home directory, and shell).

/etc/shadow: The secure file containing encrypted user passwords and account expiration details.

/etc/group: The file defining group memberships.

visudo: The command used to safely edit the /etc/sudoers file. It checks for syntax errors before saving, preventing you from being locked out of admin access.

Skeleton Directory (/etc/skel): A template directory. Files placed here are automatically copied to a new user's home directory when the account is created.

PAM (Pluggable Authentication Modules): A flexible mechanism for authenticating users. It allows admins to change authentication methods (like moving from passwords to biometrics) without rewriting software.

## 2. Text Processing and Editing
Vim / Nano: The standard text editors. You must know how to navigate, edit, and save in Vim for the performance-based questions.

Standard Streams:

stdin (0): Standard Input.

stdout (1): Standard Output.

stderr (2): Standard Error.

Piping (|): Sending the output of one command to the input of another.

Regex (Regular Expressions): Patterns used for searching and manipulating text (e.g., ^ for start of line, $ for end of line).

grep / sed / awk: The "Holy Trinity" of text processing.

grep: Search for patterns.

sed: Stream Editor (find and replace).

awk: Pattern scanning and processing (great for columns/delimited data).

## 3. System Services
Unit File: A configuration file used by systemd to define a service, mount point, or device.

Daemon: A background process that runs constantly to handle specific requests (e.g., sshd for SSH, httpd for web).

systemctl: The primary tool for controlling services (start, stop, enable, disable).

journalctl: The tool used to query and display logs from the systemd journal.

Runlevels / Targets: The state the system is in.

multi-user.target: Standard server mode (no GUI).

graphical.target: Desktop mode with a GUI.

## 4. Container Operations (The v8 Focus)
Container Engine: The software that manages containers (e.g., Podman or Docker).

Image: A read-only template used to create a container.

Container: A running, isolated instance of an image.

Dockerfile / Containerfile: A script containing instructions on how to build a container image.

Port Mapping: Linking a port on the host machine to a port inside the container (e.g., mapping host port 8080 to container port 80).

Persistent Volume: A way to store container data outside the container’s ephemeral filesystem so it isn't lost when the container stops.

## 5. Localization and Environment
Environment Variables: Values that affect the behavior of processes (e.g., $PATH, $HOME, $SHELL).

Alias: A shortcut for a long command (e.g., alias ll='ls -la').

Locale: A set of parameters that defines the user's language, country, and any special variant preferences (managed via localectl).

# Domain 4 Keywords

## 1. Bash Scripting Fundamentals
Shebang (#!): The first line of a script that tells the kernel which interpreter to use (e.g., #!/bin/bash or #!/usr/bin/python3).

Variables: Containers for data. In Bash, they are assigned without spaces (e.g., VAR="value") and accessed with a dollar sign ($VAR).

Exit Code ($?): A number between 0 and 255 returned by a command. 0 always means success; non-zero means an error occurred.

Positional Parameters: Variables used to handle arguments passed to a script (e.g., $0 is the script name, $1 is the first argument).

Conditional Logic: Using if, then, else, and fi to make decisions based on tests (e.g., [ -f file.txt ] checks if a file exists).

Loops: * for: Runs a set number of times (over a list or range).

while: Runs as long as a condition is true.

## 2. Python Scripting (New to v8)
Indentation: Unlike Bash, Python uses whitespace (tabs or spaces) to define blocks of code. Incorrect indentation causes a script to fail.

Lists & Dictionaries: Python’s primary ways to store data sets.

List: ['server1', 'server2'] (ordered).

Dictionary: {'hostname': 'web01'} (key-value pairs).

Modules: External code libraries. You’ll likely see import os or import sys in scripts that interact with the Linux system.

pip: The standard tool for installing Python packages.

## 3. Version Control (Git)
Repository (Repo): A directory where Git tracks the history of all file changes.

Commit: A "snapshot" of your code at a specific point in time.

Clone: Copying an existing remote repository to your local machine.

Push/Pull: * Push: Sending your local commits to a remote server (like GitHub).

Pull: Fetching and merging changes from a remote server to your local machine.

.gitignore: A file that tells Git which files or directories to ignore (e.g., passwords or temporary logs).

## 4. Orchestration & Infrastructure as Code (IaC)
Infrastructure as Code (IaC): Managing and provisioning infrastructure through machine-readable definition files (like YAML) rather than manual configuration.

Ansible: A popular automation tool that is agentless (uses SSH) and uses Playbooks written in YAML.

Idempotency: A key concept in automation; it means that running a script multiple times results in the same state without causing errors or duplicate changes.

YAML / JSON: The two most common data formats used for configuration files in orchestration.

## 5. AI Best Practices
Prompt Engineering: The practice of refining inputs to an AI to get more accurate scripts or troubleshooting steps.

Code Verification: The requirement to manually review and test any code generated by AI before running it in a production environment.

# Domain 5 Keywords

## 1. System Performance & Process Troubleshooting
Load Average: A measure of system utilization (CPU and I/O). Found in top or uptime, it shows the number of processes in a runnable or uninterruptible state.

Zombie Process: A process that has completed execution but still has an entry in the process table. It’s "dead" but hasn't been reaped by its parent.

OOM Killer (Out of Memory Killer): A kernel feature that sacrifices a process (usually the one using the most RAM) to prevent the entire system from crashing when memory is exhausted.

dmesg: The kernel ring buffer. This is the first place to look for hardware failures, driver issues, or "segmentation faults."

journalctl: The primary tool for querying the systemd journal. Use -u for a specific service or -p err for error-level messages.

## 2. Storage & Boot Troubleshooting
Inode Exhaustion: A condition where a disk has plenty of space but cannot create new files because it has run out of inodes (index nodes). Check with df -i.

Kernel Panic: A safety measure taken by the kernel when it encounters a fatal error it cannot recover from. Often caused by failing hardware or a bad driver.

fsck (File System Check): Used to check and repair a Linux file system. Note: Never run this on a mounted filesystem!

GRUB Rescue: The minimal shell you land in if the bootloader cannot find its configuration file or the kernel.

lsof (List Open Files): Used to identify which process is holding a file or directory open (critical when you can't unmount a drive).

## 3. Network & Security Troubleshooting
DNS Resolution: The process of turning a name into an IP. Use dig or nslookup to troubleshoot /etc/resolv.conf issues.

Latency vs. Throughput: Latency is the delay (measured by ping), while throughput is the volume of data sent over time (measured by iperf).

MTU (Maximum Transmission Unit): The largest packet size a network interface can handle. If this is misconfigured, packets may be dropped or fragmented.

ss (Socket Statistics): The modern replacement for netstat. Used to see which ports are open and which services are listening.

SELinux Contexts: Labels applied to files and processes. If a service has the right permissions but still can't access a file, it's likely an SELinux context mismatch (ls -Z).

## 4. Hardware & User Troubleshooting
Permission Denied: The most common user error. Troubleshoot by checking UGO (User, Group, Other) permissions, ACLs, or Attribute flags (lsattr).

Shared Library Issues: When a program won't start because it can't find a .so file. Use ldd to map dependencies.

sar (System Activity Reporter): Part of the sysstat package; used for long-term monitoring to find "bottlenecks" that occurred in the past.
