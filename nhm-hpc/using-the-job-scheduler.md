Using the job scheduler (Slurm)
===============================

Now you have all the files and programs you need, but there's a problem: lots of people are running huge data analyses simultaneously. That's why job scheduling systems have been developed: to ensure a fair distribution of the computing resources across all users and to allow the cluster administrators to manage such resources. HPC uses a standard open-source job scheduling system called Slurm. A complete user guide can be found on their website and ideally you should become familiar with it (https://slurm.schedmd.com/tutorials.html). 

Partitions/queues explained
---------------------------

The cluster has four 'partitions' (also known as 'queues') where you can submit your analyses (from here onwards sometimes defined as jobs) depending on their length and computational requirements: hour, day, week and month. As a general rule, the shorter the job and the less computationally expensive, the faster it gets assigned. Long analyses have lower priority.

+-----------+--------------------------------------------------------------------------------------------------------------------+
| Partition | Use case                                                                                                           |
+===========+====================================================================================================================+
| hour	    | Very short jobs, or things that can be sped up to less than an hour by multithreading                              |       
+-----------+--------------------------------------------------------------------------------------------------------------------+
| day	    | Average jobs, or things that even with multithreading can take up to 24 hours                                      |
+-----------+--------------------------------------------------------------------------------------------------------------------+
| week	    | Long jobs that take up to seven days, either because they cannot be parallelised or because they have low priority |
+-----------+--------------------------------------------------------------------------------------------------------------------+
| month	    | Non-important jobs that can run for a long time and have low priority                                              |
+-----------+--------------------------------------------------------------------------------------------------------------------+
| gpu       | For jobs that need to use the GPU. See Example 4 for an example slurm script that uses the GPU.                    |
+-----------+--------------------------------------------------------------------------------------------------------------------+


Additionally, there is an interactive partition, that can be used to test computationally intensive tasks (almost any job really) to see how they perform, with a strict control of the used resources. There are many ways of using interactive, but it should not be abused as it can reduce the priority assigned to your future analyses. This is because the scheduling system calculates the priority of your jobs depending on how much you're using the cluster, and usually when you use an interactive session you're requesting a lot of resources (most of which you aren't even using).

How to submit a job
-------------------

Example 1 - running a job using software installed with conda
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first step is to create a file that contains all the instructions for the job scheduler to run the analysis. Here is an example file which I've named slurm_script.sh.

.. image:: _images/terminal-slurm-example-1.png

The #SBATCH lines allow you to define various options for your job, as described below. Most of these options are not required, and there are many more that are not listed here, but they are important for optimization when the cluster gets busy and lots of people are submitting jobs to the same queue/partition::

    -J			job name
    -p			Partition (queue)
    -t			Time limit
    --mem			minimum memory in MB
    -c			number of CPUs
    -e			file for standard error
    -o 			file for standard output
    -w			Name of node(s) to run on
    --mail-user		who to send email notification for job state changes
    --mail-type		notify on state change: BEGIN, END, FAIL or ALL

Further options are described here: https://slurm.schedmd.com/sbatch.html. 

These lines are required if you are using conda from your home directory::

    source ~/miniconda3/etc/profile.d/conda.sh
    conda init bash

This line allows you to activate a conda environment called ``base``::

    conda activate base

The echo commands are not required, but they demonstrate how you can add additional output for your job::

    echo "Starting at `date`"
    echo "Running on hosts: $SLURM_NODELIST"
    echo "Running on $SLURM_NNODES nodes."
    echo "Current working directory is `pwd`"

Finally, this is the actual command that you're running. In this example I'm using python to run a script called python_script.py, which is located in the same directory as my slurm_script.sh. You should always write srun before your command, so the scheduler knows you're 'queueing'::

    srun python python_script.py

Use sbatch to submit your job::

    $ sbatch <script_name>

.. image:: _images/terminal-sbatch.png


.. attention::

    If your analysis produces intermediate files between steps, you should set up a scratch folder for temporary files at /mbl/share/scratch. Ideally, clean up after your job. Scratch is set to auto-delete files after 21 days.

Example 2 - running a job using software from the shared software area
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here is another sample job.

.. image:: _images/terminal-slurm-example-2.png

This time, instead of using conda, I'm using the ml command to load the AdapterRemoval software from the shared software area::

    $ ml adapterremoval

Notice that instead of having a fixed sample name/ID, you can add a variable to your .sh file. The variable NAME allows you to make the shell script more flexible, as you do not have to modify it any time you have a different sample name/ID:

    NAME=$1

Once the slurm file is ready, you just need to submit the job::

    $ sbatch slurm_script.sh SRR001

Or::

$ sbatch slurm_script.sh SRR002

If you have a text file listing all your sample names/IDs (in this case data/runids.txt), you can even use a 'for' loop to submit several jobs simultaneously (there is also a way to do it internally in the script using an array job, but that is a more complex topic)::

    $ for i in $(cat data/runids.txt); do sbatch slurm_script.sh $i; done

If there are enough computational resources the jobs will run immediately (or at least some of them), otherwise they will be placed in a queue. 

Example 3 - running a job using a singularity container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here is another sample job, this time using a singularity container:

.. image:: _images/terminal-slurm-example-3.png

This line means that we're going to run a singularity container in slurm::

    $ srun singularity run

By default, singularity can only access data in your home folder. However, you can use the --bind option to mount additional folders inside the container. This would be useful if, for example, the job needed to read or write data in the ``/workspaces/groups/rob-project-2`` folder:

    --bind /workspaces/groups/rob-project-2:/workspaces/groups/rob-project-2

This line refers to the location of the singularity image::

    ~/audiowaveform.sif

This is the application or command that we want to execute inside our container::

    audiowaveform --version

Example 4 - running a job using the GPU
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here is a sample job that makes use of the GPU. Note that only one GPU job can run at a time - if someone else's job is using the GPU, your job will wait in the queue until theirs is finished.

.. image:: _images/terminal-slurm-example-4.png

These lines are required to allow slurm to access the GPU and run on the gpu partition::

    #SBATCH --gres=gpu
    #SBATCH --partition=gpu

Like Example 3, this job runs a singularity container. The --nv flag is necessary to allow singularity to access the GPU::

    srun singularity run --nv /software/testing/biomedisa/biomedisa.sif python3 /biomedisa/biomedisa_features/pycuda_test.py


Viewing and managing your jobs
------------------------------

You can use squeue to view the status of jobs in the queue::

    $ squeue -l

.. image:: _images/terminal-squeue.png

While your job is running, you can use scontrol to check its status::

    $ scontrol show jobid <job_id>

.. image:: _images/terminal-scontrol-show.png

Once your job is finished, you can use sacct to view details of your job. This is very useful information, because it will allow you to define the exact requirements of your jobs and make good use of the computational resources::

    $ sacct --format=User,JobID,Jobname,partition,state,time,start,end,elapsed,MaxRss,MaxVMSize,nnodes,ncpus,nodelist -j <job_id>

In this example, we see the job completed successfully, it lasted five seconds, it used a maximum of 200MB of RAM, it ran on hpc-gpu-001, etc.

.. image:: _images/terminal-sacct.png

You can use sinfo to check that status of the cluster nodes::

    $ sinfo -l

In the example below we can see that hpc-gpu-001 is in a mixed state, meaning it is running jobs, but has free capacity to run more jobs. And we can see that hpc-gpu-002 is in an idle state, meaning it is not currently running any jobs, and is ready to receive new jobs.

.. image:: _images/terminal-sinfo.png

Others states you may see:
- alloc - this means the node is fully utilized by running jobs, so new jobs must wait in the queue.
- drain - this means the node is unable to to receive new jobs, either due to an error or due to scheduled maintenance.

You can cancel your running job with scancel::

    $ scancel <job_id>

Look at your own jobs in the queue::

    $ squeue -u <username>

.. image:: _images/terminal-squeue-u.png

