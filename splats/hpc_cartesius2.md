---
layout: splat
title: Using the job scheduler and working with Jupyter notebooks on Cartesius
--- 

The **job scheduling system** (or batch system) is a crucial component of every supercomputer. It ensures that usage across the vast resources of a supercomputer remains fair and follows certain guidelines (e.g. more extensive resource usage being preferred over smaller jobs, or priority of weather simulations in case of a wild-fire).

The scheduler is the gatekeeper between the login nodes, from where the user interacts with the supercomputer (installing software, up/down-loading data, ...), and the compute nodes, which execute your programmes - see image below by Dr. Gavin Pringle.

![Typical HPC system layout (figure (c) Dr. Gavin Pringle)]({{ "/splats/images/hpc2/hpc_layout.png" | absolute_url }})

## Structure of a batch script

In order to execute code on the compute nodes, one needs to communicate with the batch system. The commands depend on the system run on the supercomputer; Cartesius uses **SLURM** (formerly known as Simple Linux Utility for Resource Management).

Each job submitted to the scheduler is an **ASCII formated text file**, specifying which shell to use, as well as several special comments (`#SBATCH`). These comments inform the scheduler about the amount of resources requested (number of nodes and CPU cores, time required to run the job) and what budget to charge.

The example below is to be executed on the Bash shell (`#!/bin/bash`), requests 1 compute node (`#SBATCH -N 1`) for 15 minutes (`#SBATCH -t 0:15:00`) on the short queue (`#SBATCH -p short`, also known as partition). You can find more information about using SLURM in the [Cartesius documentation](https://userinfo.surfsara.nl/systems/cartesius/usage/batch-usage).

```bash
#!/bin/bash
#SBATCH -N 1
#SBATCH -t 0:15:00
#SBATCH -p short

# Code to be executed on the compute node(s)
echo "Hello supercomputer!"
```

**Note:** selecting your walltime requires a bit of finesse. If your requested time is shorter than the time needed to execute all your code, your job will be killed when the clock strikes zero. You will then have to re-run your job (or parts of your job). Consequently, you should select time to spare to complete the job. However, a longer walltime might have wait longer for your job to be run.

On the topic of budget: jobs are charged based on the amount of **corehours** it actually took to complete the code. If you requested a walltime of 10 hours one node with 24 cores, yet your jobs finished within 5 hours, you will only be charged 120 corehours, not 240.

**Note:** On Cartesius, your scripts will always run node-exclusive. This means even if you might only use one core to run your code, you still will be charged for the whole node.

All users will share a common budget. It is possible for a single user to blow through the budget by submitting buggy scripts (which e.g. hang and hence run infintely) with a high node and core count.


## Submitting a test job

Let's get our hands (fingertips?) dirty. On your terminal software of choice, log onto Cartesius (`ssh cartesius`) and create a folder for your project (e.g. `mkdir ~/test/`).

Copy the test script from my scripts directory to your project directory (correct the command, if you used a different folder name).

```bash
cp ~mbaron/scripts/test.sub ~/test
```

Note: `~mbaron` is another shortcut to address home directories, while `~` will be your home directory you can address other user's home directories by appending their username.

The `test.sub` (see below) requests a single node for 15 minutes on the short queue, loads Anaconda, activates the QIIME environment and prints out information about the QIIME installation.

```bash
#!/bin/bash
#SBATCH -N 1
#SBATCH -t 0:15:00
#SBATCH -p short

# Code to be executed on the compute node(s)
echo "loading modules"
module load eb
module load Anaconda3
module list
echo ""
echo "activating QIIME environment"
source activate qiime2-2019.7
echo ""
echo "printing QIIME info"
qiime info
```

**Note:** Bash's `echo` is the equivalent to Python's `print()` and statements in quotation marks will be output to the standard output. Usually this would be the screen; in this case it will be redirected to an output text file - more on this later.

To execute this job the job script needs to be submitted to the job-scheduler (SLURM). Navigate to your project directory and submit the job with `sbatch`.

```bash
cd ~/test/
sbatch test.sub
```

The batch system will prompt your submission by returning a job id and will find the right time and place to execute your job.

```example_output
[mbaron@int2 scripts]$ sbatch test.sub
Submitted batch job 6740484
```

To check progress, use `squeue`. The command by itself will print out of all jobs currently handled by the scheduler. You can limit its output with further options, such as only requesting jobs by a certain user (`$USER` is a variable containing your username, but can be replaced with a username, e.g. `mbaron`) and asking for a long report format (`-l`)

```bash
squeue -l -u $USER
```

`squeue`'s output summarises your job details, amongst them the number of nodes and time requested, current time elapsed and the state of your job (failed, pending, running, completing, completed, ...). You will mostly likely either find your jobs pending, i.e. being queued for resource allocation, or running.

**Note:** `test.sub` will likely finish quickly, so you might not catch it whilst in the queue.

```example_output
[mbaron@int2 scripts]$ squeue -l -u mbaron
Wed Aug 21 17:49:23 2019
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
           6740760     short test.sub   mbaron  RUNNING       0:01     15:00      1 tcn559
```

You can delete jobs with `scancel` followed by the job id (e.g. `6740484`). This might be handy if you accidentially submitted the wrong job, or realised that there is a mistake in the script.

```bash
scancel 6740484
```

`scancel` provides no feedback, but the job will chance its status to "completing" and eventually disappear from the queue.

## Job standard out and error out file

Each job will create a text file containing the standard and error output of the job you ran. This file will be created as soon as the job is running and follows the pattern `slurm-<job-id>.out` (e.g. `slurm-6732510.out`). The file is created in the folder from where you submitted the `sbatch` command.

You can change the name of the output file, by adding a SLURM comment to the header of your job-script. `#SBATCH demultiplex-%j.out` would, using the same job-id as above, create the filename `demultiplex-6732510.out`.

Anything the code within your job script would print to screen, including error messages, will be recorded in this file. It is an essential tool for debugging (finding and correcting errors) and you should always check the output file to make sure you job has completed appropriately.

**Note:** The liberal use of `echo` statements to print comments to your output will make debugging a lot easier.

The example below is from the test script submitted earlier. You should see that the output from the `echo` statements is interleaved with outputs from the executed programmes. If an error was thrown you could more readily pinpoint what part of the code was erroneous.

```example_output
[mbaron@int2 scripts]$ cat slurm-6740761.out
loading modules
Currently Loaded Modulefiles:
 1) bull       3) EasyBuild/3.9.2    5) eb/3.9.2(default)
 2) surfsara   4) compilerwrappers   6) Anaconda3/5.0.1

activating QIIME environment

printing QIIME info
System versions
Python version: 3.6.7
QIIME 2 release: 2019.7
QIIME 2 version: 2019.7.0
q2cli version: 2019.7.0

Installed plugins
alignment: 2019.7.0
composition: 2019.7.0
cutadapt: 2019.7.0
dada2: 2019.7.0
deblur: 2019.7.0
demux: 2019.7.0
diversity: 2019.7.0
emperor: 2019.7.0
feature-classifier: 2019.7.0
feature-table: 2019.7.0
fragment-insertion: 2019.7.0
gneiss: 2019.7.0
longitudinal: 2019.7.0
metadata: 2019.7.0
phylogeny: 2019.7.0
quality-control: 2019.7.0
quality-filter: 2019.7.0
sample-classifier: 2019.7.1
taxa: 2019.7.0
types: 2019.7.0
vsearch: 2019.7.0

Application config directory
/home/mbaron/.conda/envs/qiime2-2019.7/var/q2cli

Getting help
To get help with QIIME 2, visit https://qiime2.org
```

## Using Jupyter notebooks

Jobs on supercomputers are not run interactively, i.e. after the script has been written the user cannot interact with the compute node.

This can be tedious when it comes to data analysis, where the next step might depend on the outcome of the previous one. The use of Jupyter notebooks saves us from this continuous submission of short job scripts.

A reverse SSH tunnel enables direct access to the compute node from a login node. This connection is forwarded to the outside and accessed by your local machine via Socrates. This requires a tiny bit more SSH setup and a complicated-looking job script.

For passwordless SSH communcation between the login and compute node, generate a set of SSH-keys on Cartesius. *Do not set a passphrase!*

```bash
ssh-keygen -t rsa -b 4096
```

We then also need to add the key to the list of authorised keys.

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

Copy the Jupyter script from my scripts directory to your project directory (correct if you are using a different directory).

```bash
cp ~mbaron/scripts/jupyter.sub ~/test
```

The first part of the script is identical to the previous test script. One node is requested for the maximum of one hour of the short partition; Anaconda and QIIME are loaded. Then the script prints out instructions on how to connect to the open port of the login node, generates the reverse SSH tunnel and launches a Jupyter notebook.

```bash
#!/bin/bash
#SBATCH -t 1:00:00
#SBATCH -N 1
#SBATCH -p short

echo "loading modules"
module load eb
module load Anaconda3
module list
echo
echo "activating QIIME environment"
source activate qiime2-2019.7
echo

# randomly selecting port
PORT=`shuf -i 5000-5999 -n 1`

echo "Selected port is: " $PORT
echo
echo "To connect to Jupyter copy & paste the following command to your local terminal (not Cartesius!):"
echo
echo "ssh -N -J socrates -L${PORT}:localhost:${PORT} ${USER}@vis.cartesius.surfsara.nl"
echo
echo "Running reverse SSH tunnel"
ssh -o StrictHostKeyChecking=no -f -N -p 22 -R $PORT:localhost:$PORT int3
echo
echo "Running Jupyter lab"
jupyter lab --no-browser --port $PORT
echo
echo "Jupyter lab shut down"
```

The job-output file, e.g. `slurm-6748454.out` will contain the actual information to access the notebook running on Cartesius. Once the job is running, you will have to be patient for 1-2 minutes for the notebook to launch and all information to be printed in the output file.

```example_output
[mbaron@int2 2019]$ cat slurm-6774270.out
loading modules
Currently Loaded Modulefiles:
 1) bull       3) EasyBuild/3.9.2    5) eb/3.9.2(default)
 2) surfsara   4) compilerwrappers   6) Anaconda3/5.0.1

activating QIIME environment

Selected port is:  5874

To connect to Jupyter copy & paste the following command to your local terminal (not Cartesius!):
ssh -N -J socrates -L5874:localhost:5874 mbaron@vis.cartesius.surfsara.nl

Running reverse SSH tunnel

Running Jupyter lab
[I 15:18:11.377 LabApp] JupyterLab extension loaded from /home/mbaron/.conda/envs/qiime2-2019.7/lib/python3.6/site-packages/jupyterlab
[I 15:18:11.378 LabApp] JupyterLab application directory is /home/mbaron/.conda/envs/qiime2-2019.7/share/jupyter/lab
[I 15:18:15.468 LabApp] Serving notebooks from local directory: /nfs/home4/mbaron/2019
[I 15:18:15.468 LabApp] The Jupyter Notebook is running at:
[I 15:18:15.468 LabApp] http://localhost:5874/?token=b9f6545dfdda4276953503612628f288e886e089c160058e
[I 15:18:15.468 LabApp]  or http://127.0.0.1:5874/?token=b9f6545dfdda4276953503612628f288e886e089c160058e
[I 15:18:15.468 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 15:18:15.490 LabApp]

    To access the notebook, open this file in a browser:
        file:///nfs/home4/mbaron/.local/share/jupyter/runtime/nbserver-17767-open.html
    Or copy and paste one of these URLs:
        http://localhost:5874/?token=b9f6545dfdda4276953503612628f288e886e089c160058e
     or http://127.0.0.1:5874/?token=b9f6545dfdda4276953503612628f288e886e089c160058e

[I 11:36:51.971 LabApp] 302 GET /?token=de4fa92d33d2d22690f1dfeb4f8480c1acc308fe63731e38 (::1) 0.86ms
[W 11:36:54.563 LabApp] Could not determine jupyterlab build status without nodejs
[W 11:36:55.258 LabApp] 404 GET /api/contents/oral_microbiome.tsv?content=0&1566985015237 (::1): No such file or directory: oral_microbiome.tsv
[W 11:36:55.258 LabApp] No such file or directory: oral_microbiome.tsv
[W 11:36:55.258 LabApp] 404 GET /api/contents/oral_microbiome.tsv?content=0&1566985015237 (::1) 1.03ms referer=http://127.0.0.1:5785/lab
[W 11:36:55.261 LabApp] 404 GET /api/contents/demultiplex.ipynb?content=0&1566985015243 (::1): No such file or directory: demultiplex.ipynb
[W 11:36:55.261 LabApp] No such file or directory: demultiplex.ipynb
[W 11:36:55.261 LabApp] 404 GET /api/contents/demultiplex.ipynb?content=0&1566985015243 (::1) 0.99ms referer=http://127.0.0.1:5785/lab
[W 11:36:59.325 LabApp] Notebook test.ipynb is not trusted
[I 11:37:01.387 LabApp] Kernel started: b3cfd40a-d332-4db5-b521-fbb3d0ec0c49
[I 11:37:02.290 LabApp] Adapting from protocol version 5.1 (kernel b3cfd40a-d332-4db5-b521-fbb3d0ec0c49) to 5.3 (client).
[I 11:37:02.327 LabApp] Adapting from protocol version 5.1 (kernel b3cfd40a-d332-4db5-b521-fbb3d0ec0c49) to 5.3 (client).
[I 11:37:11.713 LabApp] Shutting down on /api/shutdown request.
[I 11:37:11.714 LabApp] Shutting down 1 kernel
[I 11:37:11.915 LabApp] Kernel shutdown: b3cfd40a-d332-4db5-b521-fbb3d0ec0c49

Jupyter lab shut down
```

First open up a new local terminal and copy paste ssh command connecting your local machine with the login node, e.g. `ssh -N -J socrates -L5316:localhost:5316 mbaron@vis.cartesius.surfsara.nl`. You will not receive any output, leave this terminal running.

Then copy and paste one of the last URLs (e.g. `http://127.0.0.1:5874/?token=b9f6545dfdda4276953503612628f288e886e089c160058e`) into your browser of choice. This should open a Jupyter lab interface where you can create a notebook.

**Note:** Without any interference, the job on Cartesius will continue executing Jupyter until the job runs out of walltime (or is cancelled manually). Hence, when you are finished make sure you shutdown Jupyter lab from its menu. This will allow the compute node to finish executing the job file and the job will complete.

### Archiving data

Your home directory on [Cartesius is limited to 200GB](https://userinfo.surfsara.nl/systems/cartesius/filesystems). Make sure you monitor your space usage frequently with the `myquota` command.

```bash
myquota
```

```example_output
[mbaron@int2 ~]$ myquota
HOME file system "/nfs/home4", disk quotas for USER mbaron:
 space   quota   limit   grace   files   quota   limit   grace
79218M    200G    240G            222k       0       0

SCRATCH file system "/scratch", disk quotas for USER mbaron:
   capacity       quota       limit      files     quota     limit [status]
    0.4064T     8.0000T     8.0000T        314   3000000   4000000 [OK]

PROJECT file system "/lustre1", disk quotas for GROUPS involving user mbaron:

PROJECT file system "/lustre2", disk quotas for GROUPS involving user mbaron:

PROJECT file system "/lustre4", disk quotas for GROUPS involving user mbaron:

PROJECT file system "/lustre5", disk quotas for GROUPS involving user mbaron:
```

Another useful programme for keeping stock of your data occupancy is the disk utility programme (`du`). It will list the file sizes of all files and folders downstream from where it is executed in kilobytes. Adding the `-h` flag will turn the output 'human readable' (K for kilo-, M for mega- and G for giga-bytes). Adding another `-s` flag will output a summary, or alternatively `-d1` will calculate the size of the top-level directories (`-d2` will step down two levels, etc.).

```bash
du -hs ~/2019/
```
```example_output
[mbaron@int2 ~]$ du -hs ~/2019/
66G	/home/mbaron/2019/
```

Running out of storage space may cause your jobs to fail. Consequently you should [archive your data](https://userinfo.surfsara.nl/systems/shared/archive-file-system), by moving it to your user's archive directory. This can be found under `/archive/username/'. This being a tape-archive, it is important that you [follow the instructions](https://userinfo.surfsara.nl/systems/shared/archive-file-system) and do not fill it up with small files (<100MB). If you have lots of smaller files for archiving, [compress them in an archive-container such as a tarball](https://www.howtogeek.com/248780/how-to-compress-and-extract-files-using-the-tar-command-on-linux/).

## Stuck?

In the first instance, please post on the Moodle forums, or come to one of the clinics to get help in person (dates to be announced).

When posting your query, please copy & paste the whole code you have issues with and, if relevant, the job output file's content.

Happy supercomputing!
