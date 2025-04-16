---
layout: default
---

# NHM Linux training

This guide is intended to introduce researchers and students at the Natural History Museum to Linux so they can run their own analyses or work on the High Performance Computing facilities more efficiently. 

### Contributors

### Contents

 - [What is Linux?](#what-is-linux)
 - [Linux Installation](#linux-installation)
   - [Windows](#windows)
   - [Mac](#mac)
 - [Basic introduction to Linux](#basic-introduction-to-linux)
 - [Installing tools](#installing-tools)
 - [Accessing High Performance Computing (HPC) facilities](#accessing-high-performance-computing-hpc-facilities)
 - [Using SLURM](#using-slurm)
 - [Dos and don'ts of working on an HPC](#dos-and-donts-of-working-on-an-hpc)
 - [Gettting help](#getting-help)

### What is Linux?
Linux is an open-source operating system, meaning it's free to use, modify, and share. It’s known for being fast, secure, and reliable, which makes it a popular choice for many bioinformaticians. 

Linux is especially useful for tasks that require high performance, such as handling large amounts of data or running complex analyses. It offers a powerful command-line interface, which allows users to automate tasks, work with scripts, and manage data more efficiently. Because it supports a wide range of software, developers and researchers can easily install and use tools for their work.

Overall, Linux is a great choice for anyone who needs a stable, flexible, and cost-effective operating system, especially for technical tasks and programming.

### Linux installation

#### Windows

To access a Linux terminal on Windows, we can use Windows Subsystem for Linux (WSL). It allows you to run a Linux distribution directly on your Windows machine.

Open PowerShell (search for "PowerShell" in the Start menu). _Note if you are on a museum laptop, you will need to open PowerShell as an Administrator_ (search for "PowerShell" in the Start menu, right-click, and select "Run as administrator").

Run the following command to enable WSL:

```
wsl --install
```

Once WSL is installed, restart your computer. 

To open a Linux terminal, open Powershell and run the following command:

```
wsl
```

It should look like this:

![Open wsl](images/open_wsl.png)

Windows also comes with an application called Terminal, which allows you to select an Ubuntu terminal.

![Open ubuntu](images/open_ubuntu.png)

#### Mac

Mac comes with a built-in terminal application that allows access to a Unix-based shell which is very similar to Linux. 

You can find it by clicking on the Spotlight search icon (top-right corner) and typing "Terminal". Alternatively, navigate to Applications > Utilities > Terminal.

### Basic introduction to Linux

Now that you have Linux installed, you are ready to get started. 

With a Linux terminal, sometimes called a _command prompt_ or _console_, you interact with your computer using only text entered via the keyboard. 

The majority of tools used in bioinformatics are designed to only work on the command line so it is important to understand how it works. 

A linux terminal lets the user interact with an application called the _shell_, which translates text into command that the computer can understand. We will focus on the most commonly used shell _BASH_. 

There are hundreds of different commands in BASH, but don't worry, you only need a handful of commands to get started. Generally, commands follow the format below:

![Command morphology](images/command_morphology.png)

Code examples can be added like this: 
```
ls -ls
```


### Useful Linux commands
| Command | Use |
| ------- | --- |
| pwd     | Print working directory (i.e. the directory you are ‘in’) |
| cd <directory/name>   | change directory |
| cd ..   | Return to previous directory/move 'back' a directory |
| cd -    | Return to the last directory path you were in |
| cd ~    | Return to home directory (i.e. /home/[your_username] |
| [TAB]   | auto-complete |
| head  <filename>  | Show the ‘head’ (i.e. top) of a particular file |

- Also see [explainshell.com](https://explainshell.com/)


### Installing tools

### Accessing High Performance Computing (HPC) facilities

Researchers at the NHM have access to two different HPC facilities: the *NHM HPC* and *Crop Diversity HPC*. Instructions for accessing both of these can be found below.

#### NHM HPC



#### Crop Diversity
- First, you'll need to [request an account to the Crop Diversity cluster](https://help.cropdiversity.ac.uk/user-accounts.html).
- 


Links to other pages can be added like this [Crop Diversity](https://help.cropdiversity.ac.uk/).

### Using SLURM

### Dos and don'ts of working on an HPC
- Be mindful of how much memory and how many CPUs you are requesting for a job. You can check resource usage of a job using the `sacct` SLURM command, and alter your memory and CPU requests for future runs accordingly.
- Don't run any code/scripts that are too computaitonally intensive, or which run longer than a few minutes, on the head node (i.e. directly on the terminal).
- Ideally, use 'scratch space' (`gpfs/nhmfsa/bulk/share/data/mbl/share/scratch/`) for storing your outputs/results. Remember 'scratch space' is not backed up, so transfer any valuable results to your 'groups' directory (`gpfs/nhmfsa/bulk/share/data/mbl/share/workspaces/groups`).
- Don't store large datasets or databases in your `home/` directory. Generally, software (such as your miniconda installation) should be stored here.


### Gettting help
