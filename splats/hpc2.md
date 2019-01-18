---
layout: splat
title: Submitting your first job script on Cirrus
--- 

The **job scheduling system** (or batch system) is a crucial component of every supercomputer. It ensures that usage across the vast resources of a supercomputer remains fair and follows certain guidelines (e.g. more extensive resource usage being preferred over smaller jobs).

The scheduler is the gatekeeper between the login nodes, from where the user interacts with the supercomputer (installing software, up/down-loading data, ...), and the compute nodes, which execute your programmes - see image below by Dr. Gavin Pringle.

![Typical HPC system layout (figure (c) Dr. Gavin Pringle)]({{ "/splats/images/hpc2/hpc_layout.png" | absolute_url }})

## Structure of a batch script

In order to execute code on the compute nodes, one needs to communicate with the batch system. The commands depend on the system run on the supercomputer; Cirrus uses **PBS Pro** (Portable Batch System Professional).

Each job submitted to the scheduler is an ASCII formated text file, starting with several special comments (`#PBS`). These comments inform the scheduler about the amount of resources requested (number of nodes and CPU cores, time required to run the job) and what budget to charge.

The example below requests 1 CPU-core on a single node for 15 minutes. The job will be charged to the budget `d411-training` and is called `example_job`. Finally, the standard and error output are combined into a single output file (makes it easier to debug mistakes). You can find more information about using PBS Pro in the [Cirrus documentation](https://cirrus.readthedocs.io/en/latest/user-guide/batch.html#using-pbs-pro).

```bash
#!/bin/bash --login
# Number of nodes (select) and cores (ncpus)
#PBS -l select=1:ncpus=1
# Walltime in hh:mm:ss
#PBS -l walltime=00:15:00
# Budget code
#PBS -A d411-training
# Job title
#PBS -N example_job
# Join standard and error out
#PBS -j oe

# Code to be executed on the compute node(s)
echo "Hello supercomputer!"
```

**Note:** selecting your walltime requires a bit of finesse. If your requested time is shorter than the time needed to execute all your code, your job will be killed when the clock strikes zero. You will then have to re-run your job (or parts of your job). Consequently, you should select time to spare to complete the job. However, a longer walltime might have wait longer for your job to be run.

On the topic of budget: jobs are charged based on the amount of **corehours** it actually took to complete the code. If you requested a walltime of 50 hours on 10 cores, yet your jobs finished within 25 hours, you will only be charged 250 corehours, not 500.

**Note:** all users within the `d411` project code share a common budget. It is possible for a single user to blow through the budget by submitting buggy scripts (which e.g. hang and hence run infintely) with a high node and core count.


## Submitting a test job

Let's get our hands (fingertips?) dirty. On your terminal software of choice, log onto Cirrus (`ssh cirrus`) and create a folder for your batch-scripts (e.g. `mkdir ~/scripts/`). Keeping your scripts in a separate folder will make it easier to add them to a git-repository for your paper.

Copy the test script from my scripts directory to your scripts directory (correct the command, if you used a different folder name).

```bash
cp ~mbaron/scripts/test.script ~/scripts
```

Note: `~mbaron` is another shortcut to address home directories, while `~` will be your home directory you can address other user's home directories by appending their username; e.g. `~gavincbm` is Gavin Pringle's home directory.

The `test.script` (see below) requests a single core for 10 minutes. It also ensures that the temporary directory for QIIME is set (the `-p` flag for `mkdir` supresses error messages if the directory already exists) and loads your QIIME installation. **This is the minimum code required to execute QIIME programmes on Cirrus.**

Eventually, the script executes the QIIME programme [`print_qiime_config.py`](http://qiime.org/scripts/print_qiime_config.html) you have previously run to test your QIIME installation.

```bash
#!/bin/bash --login
# Walltime in hh:mm:ss
#PBS -l walltime=00:10:00
# Number of nodes (select) and cores (ncpus)
#PBS -l select=1:ncpus=1
# Job title
#PBS -N test_job
# Budget code
#PBS -A d411-training
# Join standard and error out
#PBS -j oe

### start of script

echo "setting temporary directory"
mkdir -p ~/qiime_tmp
export TMPDIR=~/qiime_tmp

echo "loading virtual environment"
source deactivate
source activate qiime1

# insert your QIIME code here

print_qiime_config.py -t

### end of script
```

Navigate to your batch-scripts directory and submit the job with `qsub`.

```bash
cd ~/scripts/
qsub test.script
```

The batch system will prompt your submission by returning a job id and will find the right time and place to execute your job.

```example_output
mbaron:~/scripts$qsub test.script
404142.indy2-login0
```

To check progress, use `qstat`. The command by itself will print out of all jobs currently handled by the scheduler. You can limit its output with further options, such as only requesting jobs by a certain user as below (`$USER` is a variable containing your username, but can be replaced with a username, e.g. `mbaron`).

```bash
qstat -u $USER
```

`qstat`'s output summarises your job details, amongst them the number of nodes (`NDS`) and number of CPUs (`TSK`) requested. The `S` column informs you about the job's state. The most relevant abbreviations for you are: `Q` for queued, `R` for running, `E` for exiting (not error), `F` for finished (not failed), `H` for held. In case of `H` your likely have an error in your `#PBS` comments requesting resources.

**Note:** `test.script` will likely finish quickly, so you might not catch it whilst in the queue. You can look at jobs from your job history by using `qstat -H <job-id>`.

```example_output
mbaron:~$qstat -u shiguang

indy2-login0:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
404146.indy2-lo shiguang workq    CFAM0P9F1     --    2  72    --  15:00 Q   --
404147.indy2-lo shiguang workq    CFAM0P24F1    --    2  72    --  15:00 Q   --

mbaron:~qstat -u mbaron

indy2-login0:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
404533.indy2-lo mbaron   workq    split_benc  67310   1   1    --  01:00 R 00:06

mbaron:~$qstat -H 404533

indy2-login0:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
404533.indy2-lo mbaron   workq    split_benc  67310   1   1    --  01:00 F 00:24
```

You can delete jobs with `qdel` followed by the job id; both job number (`404533`) or full job id (`404533.indy2-login0`) will work. This might be handy if you accidentially submitted the wrong job, or realised that there is a mistake in the script.

```bash
qdel 404533
```

## Job standard out and error out file

Upon job completion an output file will be copied into the directory from which you executed the `qsub` command (the job will assume this as the working directory).

The file name is constructed with the pattern `<job_name>.o<job_id>`, e.g. `test_job.o404142`. Anything the code within your job script would print to screen, including error messages, will be recorded in this file. It is an essential tool for debugging (finding and correcting errors) and you should always check the output file to make sure you job has completed appropriately.

**Note:** Liberally use `echo` to print comments to your output. This will make debugging easier.

The example below is from the test script submitted earlier.

```example_output
mbaron:~/scripts$cat test_job.o401394
setting temporary directory
loading virtual environment

System information
==================

*snip*

QIIME base install test results
===============================
.........
----------------------------------------------------------------------
Ran 9 tests in 0.433s

OK
```

If you do not pass the `#PBS -j oe` option to the scheduler in your script-file, you will get two output files, e.g. `test_job.o401394` and `test_job.e401394` (`o` for standard out, `e` for standard error). They contain the same information, however, as your `echo` print statements are now separate from the error messages, it is somewhat more tedious to inspect the output files.

## Benchmarking a parallel programme

For large programmes/simulations with intend to use vast resources on a supercomputer, it is worthwhile to establish its parallel efficiency (see Dr. Pringle's lecture for more on the topic).

In this exercise, you will determine the parallel efficiency of one of the more resource hungry programmes of QIIME ([`pick_closed_references.py`](http://qiime.org/scripts/pick_closed_reference_otus.html)) on four datasets:

1. A full MiSeq sequencing run of ~8.7M sequences from last year and
2. cut-down datasets of 1M,
3. 100k and
4. 10k sequences of the same run.

These datasets can be found on Cirrus in the directory `~mbaron/2018/`. Each datasets consists of two compressed `fastq.gz` files, **read 1** and the **barcode/index** read. The folder also contains a mapping file for the dataset. The file paths are as follows:

```bash
~mbaron/2018/Read1.fastq.gz
~mbaron/2018/Index.fastq.gz
~mbaron/2018/Read1_1M.fastq.gz
~mbaron/2018/Index_1M.fastq.gz
~mbaron/2018/Read1_100k.fastq.gz
~mbaron/2018/Index_100k.fastq.gz
~mbaron/2018/Read1_10k.fastq.gz
~mbaron/2018/Index_10k.fastq.gz
~mbaron/2018/map.tsv
```

**Note:** Please do not copy these files, as file storage on Cirrus is limited and you do not require copies for the analysis.

### Script 1: Demultiplexing and quality filtering

In order to pick OTUs, the datasets need to be demulitplexed (re-assign sequences to samples via barcodes) and quality-filtered with the QIIME programme [`split_libraries_fastq.py`](http://qiime.org/scripts/split_libraries_fastq.html). This programme can only utilise a single CPU core/thread (serial programme), so it's best to run it separately to save resources.

Below is an example script for you to complete. Copy it to your scripts folder from `~mbaron/scripts/split_benchm.script`.

```bash
#!/bin/bash --login
#PBS -l walltime=01:00:00
#PBS -l select=1:ncpus=1
#PBS -N split_benchm
#PBS -A d411-training
#PBS -j oe

### script start

echo "setting temporary directory"
mkdir -p ~/qiime_tmp
export TMPDIR=~/qiime_tmp

echo "loading virtual environment"
source deactivate
source activate qiime1

echo "creating and cd to output directory"
mkdir -p ~/output/benchmark/
cd ~/output/benchmark/

```

The script starts with code you have seen before. However, it also creates and changes into an output directory for all the files created by `split_libraries_fastq.py`. I picked a general `output` folder within which I organise my jobs in subfolders; here `benchmark`. Choose a system you feel comfortable with.

**Note:** if you do not specifically navigate to an output directory, any output data will be written to your working directory, which is from where you submitted the job script.

```bash
# QIIME Code

echo "Demultiplexing with QIIME defaults"
echo "Full data set"
# note: the command below is broken across several lines using backslashes (\)
# you could also have the code on a single line with spaces instead
time split_libraries_fastq.py \
-i ~mbaron/2018/Read1.fastq.gz \
-b ~mbaron/2018/Index.fastq.gz \
-m ~mbaron/2018/map.tsv \
-o slout \
--rev_comp_barcode --rev_comp_mapping_barcodes

echo "1M data set"
time split_libraries_fastq.py \
-i ~mbaron/2018/Read1_1M.fastq.gz \
-b ~mbaron/2018/Index_1M.fastq.gz \
-m ~mbaron/2018/map.tsv \
-o slout_1M \
--rev_comp_barcode --rev_comp_mapping_barcodes

echo "100k data set"
# add code here

echo "10k data set"
# add code here

echo "closing environment"
source deactivate

### end script
```

The second part of the job contains the actual QIIME code. To be more efficient instead of running 4 separate jobs for each dataset, all datasets are demultiplexed and filtered consecutively.

**Note:** Each dataset has a unique output-folder defined with the `-o` tag. You will also notice that a `time` command is preceding each `split_libraries_fastq.py` command. This will output the time needed to complete the programm, our tool for benchmarking; not necessary here, but interesting nevertheless.

Edit the file and create the code needed to process the smallest two datasets, then submit your file to the job scheduler. If you forgot how to edit files with a command line editor, here is a [brief video introduction to using VIM](https://www.youtube.com/watch?v=ggSyF1SVFr4).

Then, make yourself a cup of tea or coffee, as it will take about 25 minutes to complete the job. However, do check after the kettle has boiled (`qstat -u $USER`), as most errors happen early.

Once your job has completed/exited, check the standard out and standard error log file in your scripts directory (e.g. `split_benchm.o<job_number>`). Usually error messages are pretty self-explanatory; e.g. `file/directory not found` would imply an issue with file-paths.

If no errors are reported, check your `~output/benchmark/` folder. The script should have created one `slout` directory for each dataset. Each folder should contain a log-file (read it) and a FASTA-formated `seqs.fna` file, which is the input file for OTU picking.

### Script 2: Picking OTUs

With all the input files created, it is time to write 6 benchmarking scripts. Whilst we can run a QIIME programme on several datasets within a single job script, we cannot adjust the number of cores requested on the fly (1, 2, 4, 8, 16 & 32).

The first part of the script(s) will look very familar by now. Note that `walltime` is set to 3 hours and as more cores are used this can be reduced (try 3, 3, 2, 2, 1, 1 hours for 1, 2, 4, 8, 16, 32 cores) .

```bash
#!/bin/bash --login
# reduce the walltime as more cores are requested
#PBS -l walltime=03:00:00
# to request more cores change ncpus
#PBS -l select=1:ncpus=1
#PBS -N otu_benchm_1c
#PBS -A d411-training
#PBS -j oe

### script start

echo "setting temporary directory"
mkdir -p ~/qiime_tmp
export TMPDIR=~/qiime_tmp

echo "loading virtual environment"
source deactivate
source activate qiime1

echo "creating and cd to output directory"
mkdir -p ~/output/benchmark/
cd ~/output/benchmark/
```

The second part of the script applies `BASH` for-loops to execute each programme for each dataset three-times (three datapoints for more reliable data). Besides the input (`-i`) and output (`-o`) parameters, three more parameters are passed to the programme. 

* `-f` ensures that any data already written to the output directory will be overwritten,
* while `-O -a 1` defines the number of threads/cores used by [`pick_closed_references.py`](http://qiime.org/scripts/pick_closed_reference_otus.html).

```bash
# QIIME code

echo "1 core"
echo "Picking closed reference OTUs"
echo "Full dataset"
for i in {1..3}
do
    echo "run $i"
    time pick_closed_reference_otus.py \
    -i slout/seqs.fna \
    -o otus \
    -f -a -O 1
done

echo "1M dataset"
for i in {1..3}
do
    echo "run $i"
    time pick_closed_reference_otus.py \
    -i slout_1M/seqs.fna \
    -o otus \
    -f -a -O 1
done

echo "100k dataset"
for i in {1..3}
do
    echo "run $i"
    time pick_closed_reference_otus.py \
    -i slout_100k/seqs.fna \
    -o otus \
    -f -a -O 1
done

echo "10k dataset"
for i in {1..3}
do
    echo "run $i"
    time pick_closed_reference_otus.py \
    -i slout_10k/seqs.fna \
    -o otus \
    -f -a -O 1
done

echo "closing environment"
source deactivate

### end script
```

Copy the script from `~mbaron/scripts/otu_benchm_1c.script` to your scripts directory. To create all 6 copies required at the same time, use the following short BASH script.

```bash
for i in 1 2 4 8 16 32; do cp ~mbaron/scripts/otu_benchm_1c.script ~/scripts/otu_benchm_${i}c.script; done
```

For each file, update:

* Job name (e.g `#PBS -N otu_benchm_2c` for 2 cores)
* Requested time (`walltime`),
* CPU cores (e.g. `#PBS -l select=1:ncpus=2` for 2 cores),
* CPU cores used by `pick_closed_reference_otus.py` (e.g. `-O 2` for two cores),
* Output directory used by `pick_closed_reference_otus.py` (e.g. `-o otus_2c` for two cores). This is to avoid several jobs writing to the same directory, which would surely cause troubles.
* `echo "1 core"`

Submit your jobs and wait to catch any early errors (`qstat` to check). The longest job will take a bit more than 2 hours to complete.

### Where's the data?

Your data will be contained in the job-output text-files; in particular each `real` output of the `time` command ([more information on `user` and `sys`](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)).

```example_output
mbaron:~/scripts$cat otu_benchm_1c.o404604
setting temporary directory
loading virtual environment
creating and cd to output directory
1 core
Picking closed reference OTUs
Full dataset
run 1

real	35m6.622s
user	1m43.027s
sys		0m7.354s
run 2

*snip*

closing environment
```

The commmand below will extract only the lines with the word `real` in them and save them in `data.txt`, i.e. all the time data from all output files. The output will be ordered from 1 to 32 cores and within each dataset from 8.7M to 10k sequences, each in triplicates.

```bash
for i in 1 2 4 8 16 32; do grep real otu_benchm_${i}c.o* >> data.txt; done
```

Once you collated all the data, you can calculate speedup and parallel efficiencies, as described on this slide by Dr. Pringle.

![Performance Metric (figure (c) Dr. Gavin Pringle)]({{ "/splats/images/hpc2/speedup_parallel_efficiency.png" | absolute_url }})

### Cleaning up

Cirrus' storage is a limited and shared resource. It is important to clear out data which is not needed any more. A useful programme for keeping stock of your data needs is the disk utility programme (`du`). It will list the file sizes of all files and folders downstream from where it is executed in kilobytes. Adding the `-h` flag will turn the output 'human readable' (K for kilo-, M for mega- and G for giga-bytes) and adding another `-s` flag will output a summary.

```bash
du -hs ~/output/benchmark
```

As the purpose of this benchmarking exercise was to collect time-series data (and teach you to interact with the batch-system), the intermediate outputs can now all be deleted. The command below will delete the folder and all files and sub-folders within (`-r` for recursive) and output information about this (`-v` for verbose).

```bash
rm -rv ~/output/benchmark
```

### A final note on benchmarking

Benchmarking is best carried out on a problem similar to the final application, yet feasible to run in a short time-frame on fewer resources. For short jobs, which are not repeated many times over, benchmarking would likely consume more resources than simply running the job on sub-optimal resources.

Hence, for most QIIME programmes, it is not necessarily sensible to benchmark. 


## Common mistakes in scripts

1. **Mistakes in using QIIME programmes.** Before submitting a job double check that all programmes names are spelled correctly, parameters are separated by spaces and all required inputs and outputs are defined (see documentation on [http://qiime.org/scripts/](http://qiime.org/scripts/)).
2. **Output directory already exists.** Some scripts will not overwrite existing output directories, so make sure you delete the output of previously failed jobs.
3. **Typos in filepaths, or non-existing file paths.** Double check your files are where you think they are. When using relative paths, are they correct based on where you job script will be running?
4. **Hanging script.** Most QIIME programmes should complete within a couple of hours. If you job does not, it is likely stuck on a programme bug. Do not just increase resources, but ask for help. Sometimes a programme can also get stuck if the last line of the scipt lacks a carriage return - all example scripts therefore contain a comment on the last line (`### end of script`).

## Getting help

In the first instance, please post on the Moodle forums, or come to one of the clinics to get help in person (dates to be announced).

When posting your query, please copy & paste the whole code you have issues with and the job output file's content.

Happy supercomputing!