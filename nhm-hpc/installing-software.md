Installing software
===================

You can install software in the following locations:

- Your home folder (using tools such as conda)
- In a singularity container image
- The shared software area (by compiling the software)

Each method has pros and cons as described below:


| Location | Pros | Cons |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Your home folder            | - Easy to do using tools like conda                                                                                                                 | - Space in your home folder is limited to 50GB <br>                         |
|                             |                                                                                                                                                     | - Only you have access to the software, so no one else can use it           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Singularity container image | - Allows you to create an isolated environment with all the software dependencies you need, independent of other software installed on the cluster. |                                                                             |
|                             | - Makes for a consistent and reproducible software environment.                                                                                     |                                                                             |
|                             | - You can use the same container image on other HPC clusters.                                                                                       |                                                                             |
|                             | - Other users can use the container image as well (provided the image file is in a shared location, like a group workspace).                        |                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| The shared software area    | - No limits on space                                                                                                                                | - You must compile the software, so it's more complicated and takes longer. |
|                             | - Other users can also use the software                                                                                                             |                                                                             |

In your home folder
-------------------

The easiest method of installing software is in your home folder. To do this you can use something like conda, as described below.

Installing conda
^^^^^^^^^^^^^^^^

You can download conda from https://docs.conda.io/en/latest/miniconda.html. Follow the instructions below to install it.

On hpc-head-002, change to your home folder::
  $ cd ~

Download miniconda::

  $ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

Make the file executable::

  $ chmod +x Miniconda3-latest-Linux-x86_64.sh

Install miniconda::

  $ ./Miniconda3-latest-Linux-x86_64.sh

.. image:: _images/terminal-install-conda.png

Answer yes to any questions, accept the default options, and wait for the installation to complete. Once done, close your terminal, then log back in to hpc-head-002. You should see (base) in front of your prompt. This shows that conda has been installed.

.. image:: _images/terminal-conda-prompt.png

Installing software with conda
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You are now ready to use conda to install any software you like. As an example, we will demonstrate how to use conda to install an application called R.

This command will create a conda environment called r-environment and install two groups of packages called r-essentials and r-base::

  $ conda create -n r-environment r-essentials r-base

.. image:: _images/terminal-conda-install-packages.png

It may take a few minutes to install. Once done, activate the environment. You will see your prompt change from (base) to (r-environment)::

  $ conda activate r-environment

.. image:: _images/terminal-conda-activate.png

You can use the which command to check that R is indeed installed in your home directory::

  $ which R

.. image:: _images/terminal-conda-which.png

Finally, launch R::

  $ R

.. image:: _images/terminal-conda-r.png

Singularity/Apptainer
---------------------

Singularity (sometimes called Apptainer) is a useful tool that allows you to install software inside an isolated environment known as a container. This allows you to install the software you need, without worrying about missing dependencies or conflicts with other software already installed on the cluster. 

You can learn more about it at https://apptainer.org/docs/user/latest/. 

If you've worked with docker, then this concept will be familiar to you. Singularity is commonly used on HPC systems instead of docker, as singularity can be run without admin rights. However, they are very similar to each other, and you can even convert docker images to Singularity.

Creating a singularity image 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can create a singularity image on your own computer if you have singularity installed. Alternatively, you can use our software-building server hpc-sw-003, which already has it installed (please `contact TS <TS-ServiceDesk@nhm.ac.uk>`__ to request access to this server). 

Using PuTTy or a terminal, SSH to hpc-sw-003::

  $ ssh <username>@hpc-sw-003

Create your singularity image file. You can name it anything, but it should end in .def::

  $ touch <filename>.def

Open it with a text editor and construct your file. Here is a simple example:

.. image:: _images/terminal-singularity-def.png

Here is an explanation of the different sections.

This section means use the ubuntu:22.04 image from the Dockerhub container registry. You can use other container registries and images, but ubuntu works well in many cases::

    Bootstrap: docker
    From: ubuntu:22.04

The %post section allows you to list commands to run when building the image. In this example we are installing the bwa and samtools applications inside the image::

  %post
      apt update
      apt-get install bwa samtools -y

This is a helpful label that describes what the container does::

  %help
      This is an Ubuntu container with bwa and samtools installed.

Now that you have your definition file, you can build the image. This will download the base image (ubuntu in this example), install the software (bwa and samtools), and create an image file ending in .sif::

  $ singularity build <filename>.sif <filename>.def

.. image:: _images/putty-singularity-build.png

Now that you have an image file, you can run your container using the following command. This is useful to see what you've created, as you can explore the file system and see what's installed::

  $ singularity run <filename>.sif

.. image:: _images/putty-singularity-run.png

Now that you've built your image you can upload it to the HPC cluster using sftp or scp::

  $ scp <filename>.sif <username>@hpc-head-002:~

.. image:: _images/putty-scp-singularity.png

See Example 3 for guidance on how to write your slurm script to run your singularity container on the HPC cluster.

Converting a Docker image to Singularity 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Instead of creating a singularity image from scratch, you can convert an existing docker image to singularity. 
To do this, pull the image from the container registry (normally docker hub, but could be somewhere else)::

  $ singularity pull docker://<image>

.. image:: _images/putty-singularity-pull.png

This will create an image file ending in .sif. Then you can upload it to hcp-head-002 as described in the previous section.

In the shared software area
---------------------------

If you install software in the shared software area, it means other users can also use it. There are two shared software locations, as described below.

+-----------------------+--------------------------------------------------------------------------+
| Location              | Description                                                              |
+=======================+==========================================================================+
| ``/software/testing`` | - Anyone can install software here.                                      |        
|                       | - It is useful for testing and trying out new software installations.    |
+-----------------------+--------------------------------------------------------------------------+
| ``/software/common``  | - Only superusers can install software here.                             |
|                       | - It is intended for software that has been tested and is known to work. |
+-----------------------+--------------------------------------------------------------------------+

You can check which software has already been installed in the shared software area::

  $ ml avail

.. image:: _images/terminal-ml-avail.png

You can use the spider command for more detail::

  $ ml spider

.. image:: _images/terminal-ml-spider.png

To load a piece of software into your environment::

  $ ml <software_name>/<version>

.. image:: _images/terminal-ml-load.png

When you go to the folders ``/software/testing`` or ``/software/common``, you may find more names, but if they do not appear when you run any of the commands above, they haven’t been properly installed. 

Example of installing shared software
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Next, a real example: installing AdapterRemoval. You can follow this as a guideline to install any other software. Please use this as a rough guide, as the precise steps can vary depending on the software.

Move to the ``/software/testing`` folder::

  $ cd /software/testing/

.. image:: _images/terminal-cd-software.png

Create the folder where you will keep the software::

  $ mkdir -p adapterremoval/2.3.2/{src,x86_64/bin}

Move to the folder where you will download the software::

  $ cd adapterremoval/2.3.2/src/

.. image:: _images/terminal-mkdir-software.png

Download the software (using the -O option, you can rename the downloaded file)::

  $ wget -O adapterremoval-2.3.2.tar.gz https://github.com/MikkelSchubert/adapterremoval/archive/v2.3.2.tar.gz

.. image:: _images/terminal-software-wget.png

Decompress the files::

  $ tar xvzf adapterremoval-2.3.2.tar.gz

.. image:: _images/terminal-software-untar.png

Move to the folder where the software can be compiled::

  $ cd adapterremoval-2.3.2

.. image:: _images/terminal-cd-adaptorremoval.png

Compile the software::

  $ make

.. image:: _images/terminal-make.png

Now you have a locally executable programme in ``/software/testing/adapterremoval/2.3.2/src/adapterremoval-2.3.2/build/AdapterRemoval``. Copy the executable file to the bin folder you created at the beginning::

  $ cp build/AdapterRemoval /software/testing/adapterremoval/2.3.2/x86_64/bin/

.. image:: _images/terminal-cp-build.png

Now it is necessary to create a module file which will allow you to load the software anytime you use it from any location. Create a folder to store the module file (make sure the folder name is the same as the one you created in ``/software/testing``)::

  $ mkdir -p /software/testing/modulefiles/Core/adapterremoval/

Then create the .lua file, which contains all the module information:

  $ touch /software/testing/modulefiles/Core/adapterremoval/2.3.2.lua

.. image:: _images/terminal-touch-lua.png

This is the information that should be copied into the .lua file (using a text editor like vim or nano)::

    whatis("AdapterRemoval v2.3.2 - rapid adapter trimming, identification, and read merging http://adapterremoval.readthedocs.io/")
    local name = "adapterremoval"
    local version = "2.3.2"
    local base = pathJoin("/software", "testing", name, version, "x86_64")
    prepend_path("PATH", pathJoin(base, "bin"))

You can now see your new software is available for use (you can also see there is an older version 2.3.1 available, which is fine - it means users can use either version)::

  $ ml avail adapterremoval

.. image:: _images/terminal-ml-avail-adaptor.png

Now you're ready to use your new software, so you load it into your environment::

  $ ml adapterremoval/2.3.2

You can check that it's loaded by using which::

  $ which AdapterRemoval

Finally, you can run the software::

  $ AdapterRemoval

.. image:: _images/terminal-run-adaptorremoval.png

If at some point you want to install a newer version of the software, you don’t need to delete the old one (actually it can be useful to have both). You can install the new version in the same folder, for example::

  $ mkdir -p /software/testing/adapterremoval/3.0.0/{src,x86_64/bin}

To load the old version::

  $ ml adapterremoval/2.3.2

To load the new version::

  $ ml adapterremoval/3.0.0
