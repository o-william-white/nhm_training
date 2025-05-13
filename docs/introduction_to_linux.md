---
title: Introduction to Linux
layout: default
nav_order: 3
---

# What is Linux?
Linux is an open-source operating system, meaning it's free to use, modify, and share. It’s known for being fast, secure, and reliable, which makes it a popular choice for many bioinformaticians. 

Linux is especially useful for tasks that require high performance, such as handling large amounts of data or running complex analyses. It offers a powerful command-line interface, which allows users to automate tasks, work with scripts, and manage data more efficiently. Because it supports a wide range of software, developers and researchers can easily install and use tools for their work.

Overall, Linux is a great choice for anyone who needs a stable, flexible, and cost-effective operating system, especially for technical tasks and programming.

# Basic introduction to Linux

Now that you have Linux installed, you are ready to get started. 

With a Linux terminal, sometimes called a _command prompt_ or _console_, you interact with your computer using only text entered via the keyboard. 

The majority of tools used in bioinformatics are designed to only work on the command line so it is important to understand how it works. 

A linux terminal lets the user interact with an application called the _shell_, which translates text into command that the computer can understand. We will focus on the most commonly used shell _BASH_. 


## Linux commands

There are hundreds of different commands in BASH, but don't worry, you only need a handful of commands to get started. Generally, commands follow the format below:

![Command morphology](../images/command_morphology.png)

Code examples can be added like this: 
```
ls -ls
```

Most Linux commands will have a manual page that provides the relevant information about the command use and options available. Linux manual pages are often referred to as **man** pages. To open a man page for a particular command you just need to type `man` followed by the command you are interested in. You can then browse through the man page using your cursor keys (↓ and ↑). To close the man page than hit `q` on your keyboard.

You can look up the above command like this: 

```
man ls
```

The command `ls` lists files in a directory. 
You might notice the optional argument of `-h`, this is another way to briefly check what options exist for a command. Most commands have the `-h` option available.  

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


## Working with directories

A directory is what would be considered as a "folder" if you were using most graphical user interfaces. If you think of your directory and items in it (sometimes other directories, or subdirectories) as a tree, then you can imagine moving between them by moving up or down. 


![Directories structure](../images/directories_structure.png)


The command used to change directories is `cd`.
To change to a directory one below you are in, just use the `cd` command followed by the subdirectory name:
```
cd subdir_name
```

To change directory to the one above your are in, use the shorthand for “the directory above”, "..":

```
cd ..
```

If you need to change directory without worrying where you are now, you could explicitly state the full path:
```
cd /usr/local/bin
```
If you wish to return to your home directory at any time, just type cd by itself.
```
cd
```
or 
```
cd ~
```

And finally, you can return to the last directory you were working in before this one by typing:

```
cd -
```

If you get lost and want to confirm where you are in the directory structure, use the `pwd` command which stands for print working directory. This will return the full path of the directory you are currently in. Also, by default in Bio- Linux, you see the name of the current directory you are working in as part of your prompt.

