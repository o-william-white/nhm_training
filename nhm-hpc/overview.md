Overview
========

An HPC (High Performance Computing) cluster is a collection of interconnected computers (nodes) that work together to perform complex computational tasks. These clusters are designed to provide high processing power by combining the resources of multiple nodes to tackle large-scale problems more efficiently than a single computer could. HPC clusters are commonly used in scientific research, simulations, data analysis, and other fields that require significant computing capabilities.

NHM provides an HPC cluster accessible to staff. This guide details the primary components of the cluster and offers instructions on how to utilize it.

Login node
----------

The head nodes are the nodes you log into using SSH, and are used to manage files, install software, and submit jobs through the job scheduler (Slurm). The head node is called hpc-head-002.

Compute nodes
-------------

The nodes where actual computations are performed. Jobs are distributed across these nodes to utilize their combined processing power. 

| Hostname     | CPUs | Memory | GPUs                       |               
|--------------|------|--------|----------------------------| 
| hpc-cpu-001  | 192  | 2TB    | n/a                        |
| hpc-cpu-005  | 64   | 64GB   | n/a                        |
| hpc-cpu-006  | 64   | 192GB  | n/a                        |
| hpc-cpu-007  | 192  | 2TB    | 2 X Nvidia A2 16GB         |
| hpc-gpu-001  | 192  | 2TB    | 1 x Nvidia Tesla V100 32GB |
| hpc-gpu-002  | 192  | 2TB    | 1 x Nvidia Tesla V100 32GB |


Requesting access
-----------------

Email TS-ServiceDesk@nhm.ac.uk to request access.

Technical support
-----------------

Check out the [HPC Users Community](https://teams.microsoft.com/l/channel/19%3A8aa968ad33924f1e9ac3b4a78862eac4%40thread.tacv2/General?groupId=7e89a01f-f1c5-4886-b494-0ac470650c5a>) on Microsoft Teams for assistance from fellow users and cluster admins, as well as the latest news and updates about the cluster. 

You can also contact TS via email at TS-ServiceDesk@nhm.ac.uk.

Other HPC services
------------------

Other HPC services are available to NHM staff and students, as detailed below.

### Franklin & Sanger
- This cluster is managed by TS whereas franklin and sanger are managed by Peter Foster.
- This cluster requires that analyses are submitted via a job scheduler (Slurm).
- This cluster has more compute power, and also has GPUs.

### UK Crop Diversity HPC cluster

A much larger HPC cluster that is shared with other institutions. Vist the [UK Crop Diversity](https://www.cropdiversity.ac.uk) website for more information.



