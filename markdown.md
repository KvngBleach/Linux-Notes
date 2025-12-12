# Understanding Package Managers and systemctl
  
  </table> A package manager is a command-line or graphical tool used to automate the process of installing, updating, and removing software packages on a Linux system. Software packages are collections of files, including executables, libraries, configuration files, and documentation, that are bundled together for easy distribution and installation. Package managers keep track of these packages, making it easier to manage software on a Linux system.

## Functionalities of Package Manager in Linux
  * Installation: Package managers facilitate the installation of software packages from repositories or local files. They automatically handle dependencies, ensuring that all required components are installed.
  * Dependency Resolution: Linux software often relies on other packages. Package managers detect these dependencies and fetch and install them automatically.
  * Upgrading: Package managers allow users to update installed software to the latest versions. This helps in keeping the system secure and up-to-date.
  * Removal: Uninstalling software is straightforward with package managers. They ensure that all related files and dependencies are removed, preventing conflicts.
  * Querying: Users can query the package manager to get information about installed packages, available updates, and package details.

## Most Widely used Package Manager in Linux, APT (Advanced Package Tool):

  </table> APT (Advanced Package Tool) is a powerful and widely used package manager in the Linux world. It's the default package manager for Debian-based distributions, including Debian itself, Ubuntu, and Linux Mint. APT simplifies software management, ensuring that users can easily install, update, and remove packages while taking care of dependencies. Let's explore APT in more detail, including its commands and real-world examples. 
Key Features of APT:
  
  * Dependency Resolution: APT is known for its robust dependency resolution capabilities. It automatically detects and installs any necessary libraries or packages required by the software you want to install. This ensures that the software functions correctly without manual intervention.
 
  * Repository Management: APT connects to online repositories where software packages are hosted. Users can configure which repositories to use, allowing them to install software from official sources and trusted third-party repositories.
 
  * Package Management: It allows users to manage packages on the system, including installation, upgrading, downgrading, and removal. Additionally, APT can lock packages to specific versions, ensuring system stability.
 
  * Cache Management: APT maintains a local cache of package information. This cache helps in faster package searches and updates. Users can update this cache using the apt-get update command.

 Basic commands in APT:

Installing a package:

    sudo apt-get install package-name

Updating the package list:

     sudo apt-get update
        
Upgrading packages:

    sudo apt-get upgrade
        
Removing a package:

    sudo apt-get remove package-name
      
Searching for packages:

     apt-cache search package-name

## YUM (Yellowdog Updater, Modified):

</table> YUM (Yellowdog Updater, Modified) is a package manager primarily used in Red Hat-based Linux distributions such as CentOS, Fedora, and RHEL (Red Hat Enterprise Linux). YUM has played a significant role in simplifying package management on these systems, although it's important to note that in recent years, dnf (Dandified YUM) has become the modern and recommended replacement for YUM. Here, we will discuss YUM, its commands, and provide examples of its usage.

Key Features of YUM:

* Dependency Resolution: YUM, like APT, is adept at handling complex dependency resolution. It automatically identifies and installs any necessary dependencies for the packages you want to install, ensuring that your software functions correctly.

* Repository Management: YUM interacts with software repositories to fetch and install packages. Users can configure these repositories to obtain software from both official sources and trusted third-party repositories.

* Package Management: YUM provides a set of commands to manage packages on the system. This includes installing, updating, downgrading, and removing packages, as well as listing installed packages and their information.

* Cache Management: YUM maintains a local cache of metadata from the repositories. This cache improves package search and installation speed. You can update this cache using the yum makecache command.

### Basic commands in YUM:

Installing a package:

    sudo yum install package-name
    
Updating the package list:

    sudo yum makecache
    
Upgrading packages:

    sudo yum update
    
Removing a package:

    sudo yum remove package-name

## dnf Package Manager

</table> dnf is the successor to YUM and is now the recommended package manager for Fedora-based distributions. It provides a more modern and user-friendly experience. The syntax and commands for dnf are similar to yum.

Some dnf examples are:

Installing a package:

    sudo dnf install package-name
    
Updating packages:

    sudo dnf upgrade

## Pacman

</table> Pacman is the package manager used in Arch Linux and its derivatives, such as Manjaro. Arch Linux is known for its simplicity, flexibility, and rolling-release model, and Pacman is a vital component that makes managing software on Arch-based systems efficient. In this section, we'll explore Pacman, its commands, and provide examples of its usage.

Key Features of Pacman:

* Simplicity and Transparency: Pacman follows a simple and transparent approach to package management. It doesn't hide the details and allows users to understand precisely what's happening during package installation, upgrade, and removal.

* Rolling Release: Arch Linux and its derivatives use a rolling-release model, meaning that software is continuously updated rather than waiting for major distribution releases. Pacman plays a crucial role in keeping the system up-to-date with the latest software versions.

* User-Managed Configuration: Pacman gives users significant control over their system's configuration files. It provides options to save, overwrite, or merge configuration files during package upgrades, ensuring that the system stays tailored to the user's preferences.

* AUR Support: The Arch User Repository (AUR) is a community-driven repository for additional software not available in the official repositories. Pacman can be extended to manage packages from the AUR with AUR helpers like yay.

Basic commands in Pacman:

Installing a package:

    sudo pacman -S package-name
    
Updating the package list and upgrading packages:

    sudo pacman -Syu\
    
Removing a package:

    sudo pacman -R package-name
    
Querying package information:

    pacman -Q package-name

## Zypper:

</table> Zypper is the default package manager used in openSUSE and its derivatives. It plays a critical role in managing software packages on these Linux distributions. Zypper is known for its efficiency and robust dependency resolution capabilities. In this section, we will delve into Zypper, its commands, and provide examples of its usage.

Key Features of Zypper

* Dependency Resolution: Zypper is highly effective at handling dependencies. It automatically detects and resolves package dependencies, ensuring that all required components are installed, which is crucial for software to function correctly.

* Repository Management: Zypper interacts with software repositories, making it easy to fetch and install packages. openSUSE provides a wide range of official and community repositories, giving users access to a vast selection of software.

* Package Management: Zypper offers a range of package management commands, including installation, upgrading, downgrading, and removal. It provides users with control over their software packages.

* Transaction Safety: Zypper is designed with a focus on transaction safety. This means it will attempt to maintain a consistent system state by either completing a transaction successfully or rolling back changes in the event of an error, avoiding a partially updated system.

Basic commands in Zypper:

Installing a package:

    sudo zypper in package-name

Updating packages:

    sudo zypper up

Removing a package:

    sudo zypper rm package-name
    
Searching for packages:

    zypper se package-name

## DPKG (Debian Package Manager):

</table> DPKG (Debian Package Manager) is a fundamental package management tool for Debian-based Linux distributions, including Debian itself, Ubuntu, and their derivatives. DPKG is a low-level package manager, and while it directly interacts with individual package files, it is often used in conjunction with higher-level package managers like APT for a more user-friendly experience. In this section, we'll explore DPKG, its commands, and provide examples of its usage.

Key Features of DPKG: 

* Low-Level Package Management: DPKG directly handles the installation, removal, and management of individual package files (with the .deb extension). It does not automatically resolve or manage dependencies like higher-level package managers such as APT.

* Package Installation: DPKG is responsible for installing software packages on a Debian-based system. Users can install packages from local .deb files or by specifying package names if the packages are already present on the system.

* Package Removal: DPKG can remove installed packages from the system, ensuring that configuration files are retained or purged based on user preferences.

* Package Querying: Users can query the status and information about installed packages, which is useful for diagnosing issues or verifying package details.

Basic commands in DPKG: 

Installing a package from a .deb file:

    sudo dpkg -i package.deb
    
Removing a package:

    sudo dpkg -r package-name
    
Querying package information:

    dpkg -l | grep package-name

## systemctl in Linux

</table> In this article, we will explore the systemctl command, an essential tool for managing services, units, and the entire systemd ecosystem in Linux. We must also know about systemd in Linux.

What is systemd?

</table> Systemd is a system and service manager for Linux operating systems. It is designed to replace traditional init systems like SysV init and Upstart and is responsible for initializing the system, starting services, managing daemons, and monitoring system state. Its approach to managing processes is both efficient and powerful. 

* Parallel startup of services, which accelerates the boot process.
* Dependency management, ensuring that services are started in the right order.
* Cgroups integration for resource management.
* Enhanced logging through the journal system.
* Socket activation, which allows services to be started on-demand.

## Introduction to systemctl
</table> systemctl is the primary command-line interface for interacting with systemd. It provides a wide range of functions for managing services, viewing their status, and controlling the system's behavior. Let's explore some of the most common systemctl commands and their usage.

## Basic systemctl Commands

### Starting and Stopping Services
To start a service, you use the start command. For example, to start the Apache web server:

    sudo systemctl start apache2

To stop a service, use the stop command:

    sudo systemctl stop apache2

### Enabling and Disabling Services

To ensure a service starts at boot, you enable it using the enable command:

    sudo systemctl enable apache2

To disable a service from starting at boot:

    sudo systemctl disable apache2

### Restarting and Reloading Services

To restart a service:

    sudo systemctl restart apache2
    
To reload configuration files without stopping the service:

    sudo systemctl reload apache2

### Checking Service Status
To view the status of a service, use the status command:

    systemctl status apache2

### Viewing Active Units
You can list all currently active units (services, sockets, targets, etc.) with:

    systemctl list-units --type=service

# Linux File Hierarchy Structure

</table> The Linux File Hierarchy Structure or the Filesystem Hierarchy Standard (FHS) defines the directory structure and directory contents in Unix-like operating systems. It is maintained by the Linux Foundation. 

* In the FHS, all files and directories appear under the root directory /, even if they are stored on different physical or virtual devices.
* Some of these directories only exist on a particular system if certain subsystems, such as the X Window System, are installed.
* Most of these directories exist in all UNIX operating systems and are generally used in much the same way; however, the descriptions here are those used specifically for the FHS and are not considered authoritative for platforms other than Linux.

## Root 

</table> At the top of every Linux file system is the root directory represented by a forward slash /. It’s the base point, and no directory exists above it. If you look at the file system graphically, you’ll see all other directories branching from this root directory.

* Every single file and directory start from the root directory.
* The only root user has the right to write under this directory.
* /root is the root user’s home directory, which is not the same as /

</table> Only the root user has permission to modify contents inside this directory. Regular users cannot make changes here. For example, if you attempt to create a file in / as a non-root user, you’ll encounter a "Permission Denied" error.

## Bin

</table> The /bin directory contains essential commands and binaries needed by all users, including cp, ls, ssh, and kill. These commands are universally available across user types.

* Contains binary executables.
* Common linux commands you need to use in single-user modes are located under this directory.
* Commands used by all the users of the system are located here e.g. ps, ls, ping, grep, cp 

## Boot

</table> This directory stores all files required for booting the system. It includes the GRUB bootloader configuration and essential kernel files that are loaded during startup. 

* This directory stores all files required for booting the system. It includes the GRUB bootloader configuration and essential kernel files that are loaded during startup.
* Example: initrd.img-2.6.32-24-generic, vmlinuz-2.6.32-24-generic

## Dev

</table> `Device files in Linux are stored in the /dev directory. These are special files that act as interfaces between hardware and software. Device files are of two types: block devices (e.g., hard drives) and character devices (e.g., microphones and speakers). Examples include /dev/sda1 for disk partitions.

* These include terminal devices, usb, or any device attached to the system.
* Example: /dev/tty1, /dev/usbmon0

## Etc 

</table> Short for "Editable Text Configuration," /etc contains configuration files for system applications, users, services, and tools or it contains the Host-specific system-wide configuration files. For example, user details like UID and local addresses are defined here.

* This also contains startup and shutdown shell scripts used to start/stop individual programs.
* Example: /etc/resolv.conf, /etc/logrotate.conf.

## Home 

</table> Every non-root user has a personal directory inside /home. For example, if your username is anshu, your personal directory would be /home/anshu. Each user can create, delete, or modify files only in their own home directory and cannot access others’ home directories.

* Home directories for all users to store their personal files, containing saved files, personal settings, etc..
* example: /home/kishlay, /home/kv

## Lib 

</table> Applications require shared libraries to run, which are stored in /lib. These include dynamic libraries needed during runtime. For example, Apache server libraries are available here.

* Library filenames are either ld* or lib*.so.*
* Example: ld-2.11.1.so, libncurses.so.5.7

## Media

</table> Devices like USBs, CDs, and pen drives are mounted under /media. For example, when a CD-ROM is inserted (appeared in FHS-2.3), its details will appear here.

* Temporary mount directory for removable devices.
* Examples, /media/cdrom for CD-ROM; /media/floppy for floppy drives; /media/cdrecorder for CD writer

## MnT

* When external drives are connected, they are temporarily mounted in /mnt. This is where their contents become accessible to the system.
* Temporary mount directory where sysadmins can mount filesystems.

## /opt : 
Third-party software and packages not part of the default system installation are stored in /opt. It includes their configuration and data files.

* Contains add-on applications from individual vendors.
* Add-on applications should be installed under either /opt/ or /opt/ sub-directory.

## /sbin : 
Essential system binaries, e.g.,

This directory holds administrative binaries like iptables, firewall management tools, fsck, init, route etc. These binaries are primarily for system administrators and typically require root privileges to execute.

* Just like /bin, /sbin also contains binary executables.
* The linux commands located under this directory are used typically by system administrators, for system maintenance purposes.
* Example: iptables, reboot, fdisk, ifconfig, swapon

## /srv : 
Site-specific data served by this system, such as data and scripts for web servers, data offered by FTP servers, and repositories for version control systems.

* srv stands for service.
* Contains server specific services related data.
* Example, /srv/cvs contains CVS related data.
