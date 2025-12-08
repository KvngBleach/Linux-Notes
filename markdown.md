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

### Basic commands in APT:

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
