---
layout: default
---

# NHM Linux training

This guide is intended to introduce researchers and students at the Natural History Museum to Linux so they can run their own analayses or work on the High Performatnce Computing facilities more effienctly.

### Contents

 - [What is Linux?](#what-is-linux)
 - [Linux Installation](#linux-installation)
 - [Basic introduction to Linux](#basic-introduction-to-linux)
   - [Windows](#windows)
   - [Mac](#mac)
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
Code examples can be added like this: 
```
ls -ls
```

### Installing tools

### Accessing High Performance Computing (HPC) facilities

Links to other papges can be added like this [Crop Diversity](https://help.cropdiversity.ac.uk/).

### Using SLURM

### Dos and don'ts of working on an HPC

### Gettting help