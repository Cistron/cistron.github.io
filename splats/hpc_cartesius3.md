---
layout: splat
title: Exploring changes in the human oral microbiome with QIIME2
--- 

This workshop will guide you through a basic data processing pipeline using QIIME2 on an oral microbiome dataset. A large proportion will be exploratory data analysis, followed by some more specific statistical correlation analysis and plotting of data.

To work through this workshop, you should have an account on the Dutch supercomputer Cartesius and completed all the necessary setup, as instructed in the workshops on [Accessing the Supercomputer (Cartesius) and installing QIIME]({{ "/splats/hpc_cartesius1" | absolute_url }}) and [Using the job scheduler and working with Jupyter notebooks on Cartesius]({{ "/splats/hpc_cartesius2" | absolute_url }}) 

## Copying the Jupyter notebook and jobscript

Log onto Cartesius and create a working directory.

```bash
# for example
mkdir ~/qiime2_workshop
```

Copy the `qiime2_workshop.ipynb` and `jupyter.sub` to your working directory.

```bash
# for example
cp -v /home/mbaron/qiime2_workshop/qiime2_workshop.ipynb /home/mbaron/qiime2_workshop/jupyter.sub ~/qiime2_workshop
```

Have a look inside the `jupyter.sub` jobscript (e.g. using `cat` or `less`). You will notice that the wallclock request is set for 2 hours.

For today's workshop (1 Oct 2019, 11:00 to 13:00), 85 compute nodes have been reserved for our exclusive use. This means your job will be immediately executed and will not end up in a queue.

To access the reservation, the job sumission command has to be slightly modified.

```bash
# make sure you are in your working directory
sbatch --reservation=cbmedtrain jupyter.sub
```

Once submitted, check whether your job is running. **Note:** The reservation is for the whole cohort, so do not submit more than one job.

```bash
squeue -lu $USER
```

When your job *is* running, a job output file (e.g. `slurm-6748454.out`) will appear in your working directory. Wait 1-2 minutes for Jupyter to fully load on the compute node, then read the file contents (e.g. `cat slurm-6748454.out`) and follow the same steps as in the previous workshop - [Using the job scheduler and working with Jupyter notebooks on Cartesius]({{ "/splats/hpc_cartesius2" | absolute_url }}).

Further instructions for the workshop are embedded in the Jupyter notebook.