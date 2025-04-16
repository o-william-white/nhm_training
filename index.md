---
layout: default
---

# NHM Linux training

This guide is intended to introduce researchers and students at the Natural History Museum to Linux so they can run their own analayses or work on the High Performatnce Computing facilities more effienctly.

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
 - [Installing Tools](#installing-software-with-package-managers)
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

Now you have a Linux installed, you are ready to get started. 

With a Linux terminal, sometimes called a _command prompt_ or _console_, you interact with you computer using only text entered via the keyboard. 

The majority of tools used in bioinformatics are designed to only work on the command line so it is important to understand how it works. 

A linux terminal lets the user interact with an application called the _shell_, which translates text into command that the computer can understand. We will focus on the most commonly used shell _BASH_. 

There are hundreds of different commands in BASH, but don't worry, you only need a handful of commands to get started. Generally, commands follow the format below:

![Command morphology](images/command_morphology.png)

Code examples can be added like this: 
```
ls -ls
```


### Accessing High Performance Computing (HPC) facilities

Researchers at the NHM have access to two different HPC facilities: the *NHM HPC* and *Crop Diversity HPC*. Instructions for accessing both of these can be found below.

#### NHM HPC



#### Crop Diversity



Links to other papges can be added like this [Crop Diversity](https://help.cropdiversity.ac.uk/).

### Using SLURM

### Dos and don'ts of working on an HPC

### Installing software with package managers

#### `mamba` , `conda`, and `pip`
- `conda` is a system package manager 
	- you can install packages in their own environments, so that they always run as expected (In theory! As with all things, sometimes you need to debug)
	- it's good for reproducibility to have set spaces where things work
	- it also makes it easy to tell others (and future you!) how things were set up to run
- `mamba` is a drop-in replacement and uses the same commands and configuration options as `conda`. 
	- You can swap almost all commands between `conda` & `mamba` but you will need to edit a config file (not hard to do just a bit of hassle)
	- this is really optional!
	- `mamba` is a reimplementation of the conda package manager in `C++`, meaning it resolves dependencies MUCH faster
		- a dependency is a package that the package you're trying to install depends on
- If you know about `pip`
	- `conda` (and thus `mamba`) is a system package manager. `pip` is a `python` package manager. With `conda` you can install much more than just `python` libraries
	- documentation for `pip` is on its way, watch this space!
- Generally, it isn't a good idea to mix `conda`/`mamba` with `pip` (as in, use either `pip` or `conda`, or use `pip` or `mamba`)
	- however, you can create a `conda` env and install `pip` in that if you HAVE to install a package which is only `pip` compatible
	- remember, ***one conda env per main package used***! See below.
	- if multiple packages need `pip`, *they each get their own envs*
	- see below about package installs

**`mamba` documentation and helpful pages**
- A nice [page](https://statistics.berkeley.edu/computing/conda) from UC Berkeley on `conda` and `mamba` 
- `mamba`'s [manual](https://fig.io/manual/mamba) (or manpage)
- `mamba`'s [documentation](https://mamba.readthedocs.io/en/latest/index.html)
- `mamba`'s [github](https://github.com/mamba-org/mamba) page 

#### **Installing conda (and mamba if you like)**
**Installing conda**
- for the NHM cluster, you will need to do this
```
# get the installer
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# make it executable
chmod +x Miniconda3-latest-Linux-x86_64.sh

# run installer
./Miniconda3-latest-Linux-x86_64.sh

# you will have to agree to the Ts and Cs, and enter for all other things (where it's installing, name of basic stuff. keep default)

# once it's installed, log out of the cluster, then log back in
# once logged in, before the user name, you should see (base) appear
```
- the [cropdiversity](https://www.cropdiversity.ac.uk/) cluster uses [bioconda](https://help.cropdiversity.ac.uk/bioconda.html#bioconda) so if you need to use cropdiversity, see their own help pages

**Using `conda` to install `mamba`**
- most clusters do not have `mamba` as default, so you need to make sure `conda` can install it
- you might need to activate `conda`; see the docs for the HPC you're working on for how to do this (short guide above)
- on a computing cluster, `conda` should already be somewhat set up, so don't worry too much about `yaml` files and `.rc` files

```{bash}
## prioritize 'conda-forge' channel
conda config --add channels conda-forge

## update existing packages to use 'conda-forge' channel
conda update -n base --all

## install 'mamba'
conda install -n base mamba
```
- note: `mamba` should be installed into the `base` environment
	- this is the most basic env in the cluster
	- ***NO OTHER PACKAGES SHOULD BE HERE!***
	- see[ this page](https://mamba.readthedocs.io/en/latest/user_guide/troubleshooting.html#no-other-packages-should-be-installed-to-base) in the mamba docs for clarification

**a note on your `.condarc**
- Bear in mind that if there is a `.condarc` file in your `$HOME` directory, the bare minimum for it should look like this
```
channels:
  - conda-forge
  - bioconda
channel_priority: strict
```
- the main point being that `channel priority` should be strict only, not flexible, or often packages will install 'wrong' and not work
- also, there was some issue a few years ago with certain channels and licensing for academic use. DO NOT ADD A CHANNEL CALLED DEFAULT! See this [nice summary by DataCamp](https://www.datacamp.com/blog/navigating-anaconda-licensing).

#### **How to manage packages!**

***You should make sure that each package is in its own `conda` env!!!***
- this is to avoid clashes in dependencies, which can be a big risk with `conda`/`mamba`  environments
- You will still be using `conda` to activate and deactivate, but `mamba` for install (see note above)

**Where to find packages**
- most packages will be listed on [anaconda.org](https://anaconda.org/)
	- you can just search them there
- Or, the [github](https://github.com/) page for the package will have a `conda` install 

**Package install code**
- you can either `create` a new environment and then `install` into that environment once it's active, OR you can `create` and `install` in one line
- ***you MUST create envs every time you want a new package***
	- this is to avoid clashes in dependencies, which can be a big risk with `conda`/`mamba`  environments
- If a package install instruction says `conda`, you can just replace with `mamba`
```{bash}
mamba create -n <env_name> -c <channel> <package_name>
```
- options here:
	- -n = name of env (up to you)
	- -c = `conda` channel
		- this is where the package comes from
	- positional argument at end/as part of -c option = name of package to install (not up to you)

**Removing packages**
- mamba [doc](https://fig.io/manual/mamba/uninstall) for uninstall
```
mamba uninstall <package_name> -n <env_name>
```

**Activating and deactivating environments**
- you must ***activate*** an environment to use the package inside it
- to use a different package, you must deactivate the first environment and activate the next one
```
# activate conda env
conda activate <env>

# deactivate conda env
conda deactivate # do not specify env

# deactivate then activate
conda deactivate && conda activate <env> # && is a bash thing, means 'do the next thing if the first one worked successfully'
```

#### **a final note on envs and packages**
- you generally make new envs for each package
- this is to avoid clashes in dependencies, which can be a big risk with `conda`/`mamba`  environments
- however, sometimes you have to break that rule (like if code uses one package, but the output pipes to another package)
- then you can put another package into the same env (consider this a manually-added dependency)
- BUT YOU MUST BE CAREFUL. READ THE DOCS EVERY TIME TO MAKE SURE THAT EACH PACKAGE'S DEPENDENCIES DO NOT CLASH
	- e.g., package 1 relies on package a v1.0.0  to run, but you need package 2, which relies on package a v.1.0.1. It's annoying if this happens. in this case, do not install package 2 into package 1's env! new env needed!!!!!

### Getting help
