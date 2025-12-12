# Introduction to Linux Shell and Shell Scripting

Using any modern operating system like Linux, macOS, or Windows involves interacting with a shell, which interprets and executes user commands. In Linux, this interaction typically happens through the terminal, where users type commands for the shell to process.

Before learning shell scripting, you must understand:

* Kernel
* Shell
* Terminal

## Kernel
The kernel is a computer program that is the core of a computer's operating system, with complete control over everything in the system. It manages the following resources of the Linux system:

* File management
* Process management
* I/O management
* Memory management
* Device management etc.
Complete Linux system = Kernel + GNUsystem utilities and libraries + other management scripts + installation scripts.

Shell
A shell is a special user program that provides an interface for the user to use operating system services. Shell accepts human-readable commands from users and converts them into something which the kernel can understand. It is a command language interpreter that executes commands read from input devices such as keyboards or from files. The shell gets started when the user logs in or starts the terminal.

Shell is broadly classified into two categories –

* Command Line Shell
* Graphical shell

### Command Line Shell
Shell can be accessed by users using a command line interface. A special program called Terminal in Linux/macOS, or Command Prompt in Windows OS is provided to type in the human-readable commands such as "cat", "ls" etc. and then it is being executed. The result is then displayed on the terminal to the user. A terminal in Ubuntu 25.04 system looks like this:

