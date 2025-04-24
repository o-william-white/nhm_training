---
title: Accessing HPC resources
layout: default
nav_order: 1
---

Insert text here about each HPC, which might be right for each user. 

### Dos and don'ts of working on an HPC
- Be mindful of how much memory and how many CPUs you are requesting for a job. You can check resource usage of a job using the `sacct` SLURM command, and alter your memory and CPU requests for future runs accordingly.
- Don't run any code/scripts that are too computaitonally intensive, or which run longer than a few minutes, on the head node (i.e. directly on the terminal).
- Ideally, use 'scratch space' (`gpfs/nhmfsa/bulk/share/data/mbl/share/scratch/`) for storing your outputs/results. Remember 'scratch space' is not backed up, so transfer any valuable results to your 'groups' directory (`gpfs/nhmfsa/bulk/share/data/mbl/share/workspaces/groups`).
- Don't store large datasets or databases in your `home/` directory. Generally, software (such as your miniconda installation) should be stored here.