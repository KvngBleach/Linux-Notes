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
* Device management etc.To access the terminal, simply search in search box "terminal" and double-click it.
Complete Linux system = Kernel + GNUsystem utilities and libraries + other management scripts + installation scripts.

## Shell
A shell is a special user program that provides an interface for the user to use operating system services. Shell accepts human-readable commands from users and converts them into something which the kernel can understand. It is a command language interpreter that executes commands read from input devices such as keyboards or from files. The shell gets started when the user logs in or starts the terminal.

Shell is broadly classified into two categories –

* Command Line Shell
* Graphical shell

### Command Line Shell
Shell can be accessed by users using a command line interface. A special program called Terminal in Linux/macOS, or Command Prompt in Windows OS is provided to type in the human-readable commands such as "cat", "ls" etc. and then it is being executed. The result is then displayed on the terminal to the user. A terminal in Ubuntu 25.04 system looks like this:

* In the above screenshot, the ls -l command is executed. This command lists all files and directories in the current working directory in a long listing format, displaying detailed information such as permissions, ownership, file size, and modification date.
* Working with the command-line interface (CLI) can be challenging for beginners since it requires memorizing numerous commands. However, it is an incredibly powerful tool — it allows users to store multiple commands in a file and execute them together, enabling automation of repetitive tasks. These files are known as batch files in Windows and shell scripts in Linux and macOS systems

## Graphical Shells
Graphical shells provide means for manipulating programs based on the graphical user interface (GUI), by allowing for operations such as opening, closing, moving, and resizing windows, as well as switching focus between windows. Window OS or Ubuntu OS can be considered as a good example which provides GUI to the user for interacting with the program. Users do not need to type in commands for every action. A typical GUI in the Ubuntu system:

There are several shells available for Linux systems like:

* BASH (Bourne Again Shell): It is the most widely used shell in Linux systems. It is used as default login shell in Linux systems and in macOS. It can also be installed on Windows OS.
* CSH (C Shell): The C shell's syntax and its usage are very similar to the C programming language.
* KSH (Korn Shell): The Korn Shell was also the base for the POSIX Shell standard specifications etc.

Each shell does the same job but understands different commands and provides different built-in functions.

## Terminal

A program which is responsible for providing an interface to a user so that he/she can access the shell. It basically allows users to enter commands and see the output of those commands in a text-based interface. Large scripts that are written to automate and perform complex tasks are executed in the terminal.

* To access the terminal, simply search in search box "terminal" and double-click it.
* Here you can see how the terminal looks of Red Hat Linux.

## Getting Started with Shell Scripting 

In the world of DevOps and system administration, automating repetitive tasks is key to saving time and reducing errors. That’s where shell scripting comes in. It allows you to write a series of commands in a file and run them all at once, just like a program. Whether it is setting up servers, managing files, or deploying code shell scripts make it fast, consistent, and effortless.

### Shell Script
A shell script is a file containing a series of commands for the shell to execute. The shell itself is a command-line interpreter (CLI), while a shell script is a saved list of instructions (usually with .sh extension) like myscript.sh.

### Uses of Shell Scripts
Here are the main reasons shell scripts are widely used in IT and DevOps:

1. Automate Repetitive Tasks
Shell scripts turn routine jobs like file cleanup, backups, or server updates into one-click actions, saving time and reducing manual effort

2. Reliable and Consistent
Shell scripts run the exact same steps every time, without mistakes. Once you write and test a script, you can count on it to perform tasks consistently whether it's setting up servers, cleaning up files, or running a deployment. This removes human error and ensures predictable results.

3. Native Support on Unix/Linux Systems
Shell scripting is natively supported on Unix-based systems, requiring no additional installations. The shell environment (like Bash or Zsh) is pre-installed, allowing scripts to run seamlessly across most Linux distributions and Unix-like operating systems.

4. Great for Linking Commands to Work Together
Shell scripts make it easy to link different commands together. You can pass the output of one command into another, filter data, or run tasks step by step all in one script. This helps automate complex jobs using simple building blocks.

5. Ideal for IT, DevOps & Monitoring
Shell scripts help automate important tasks like checking system health, setting up servers, and scheduling jobs to run at specific times (using tools like cron). In DevOps, they are widely used to speed up deployments and manage infrastructure without doing everything manually.

6. Lightweight and Easy to Write
They are interpreted, so there is no compile step. You can write a useful script in minutes, and test it instantly perfect for quick fixes and small tasks.

## Understanding Shell Scripts: Structure, Syntax, and Usage

Shells are interactive, meaning they take user input and execute commands. But typing multiple commands repeatedly is inefficient.

Instead, you can store commands in a file a shell script and run them when needed. These scripts are similar to Windows batch files and typically have a .sh extension.

If you are familiar with languages like Python or C, you will find shell scripting easy to grasp. A shell script includes:

* Shell Keywords: if, else, break, etc.
* Shell Commands: cd, ls, echo, pwd, touch, etc.
* Functions
* Control Flow: if..then..else, case, loops, etc.

It is quite frustrating, so why not we can have a utility where we just have to type the name of directory and we can directly jump to that without executing the "cd ../" command again and again. Save the script as "jump.sh"

# !/bin/bash

# A simple bash script to move up to desired directory level directly

function jump()
{
    # original value of Internal Field Separator
    OLDIFS=$IFS

    # setting field separator to "/"
    IFS=/

    # converting working path into array of directories in path
    # eg. /my/path/is/like/this
    # into [, my, path, is, like, this]
    path_arr=($PWD)

    # setting IFS to original value
    IFS=$OLDIFS

    local pos=-1

    # ${path_arr[@]} gives all the values in path_arr
    for dir in "${path_arr[@]}"
    do
        # find the number of directories to move up to
        # reach at target directory
        pos=$[$pos+1]
        if [ "$1" = "$dir" ];then

            # length of the path_arr
            dir_in_path=${#path_arr[@]}

            #current working directory
            cwd=$PWD
            limit=$[$dir_in_path-$pos-1]
            for ((i=0; i<limit; i++))
            do
                cwd=$cwd/..
            done
            cd $cwd
            break
        fi
    done}

For now, we cannot execute our shell script because it does not have permissions. We have to make it executable by typing following command –

    $ chmod +x path/to/our/file/jump.sh
Now to make this available on every terminal session, we have to put this in ".bashrc" file.

".bashrc" is a shell script that Bash shell runs whenever it is started interactively. The purpose of a .bashrc file is to provide a place where you can set up variables, functions, and aliases, define our prompt, and define other settings that we want to use whenever we open a new terminal window.

Now open the terminal and type the following command:

    $ echo “source ~/path/to/our/file/jump.sh”>> ~/.bashrc
Now open your terminal and try out new "jump" functionality by typing following command-

    $ jump dir_name
