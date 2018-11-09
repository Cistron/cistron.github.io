---
layout: splat
title: HPC - Accessing Cirrus and installing QIIME
---

As mentioned during the course introduction, you will get access to Cirrus, a high performance compute cluster managed by the [EPCC in Edingburgh](https://www.epcc.ed.ac.uk/). This guide will walk you through account setup, setup of ssh-keys and software installation.

## Getting an Tier2 SAFE account and Cirrus login account

Navigate to the Tier2 SAFE at [https://www.archer.ac.uk/tier2/](https://www.archer.ac.uk/tier2/) and click on `create an account`.

![SAFE login page]({{ "/splats/images/hpc1/safe.png" | absolute_url }})

Fill in the form, using your UCL email address and UCL's address. Unfortunately, the list of institutions is not alphabetically sorted; type `trinity` to get close to UCL. Make sure to leave the `Opt out of user Emails` box unchecked, else you will not receive important updates from the system administrators!

![SAFE account creation]({{ "/splats/images/hpc1/safe_account_creation.png" | absolute_url }})

Once your data is processed, you will receive an email informing you of your account creation and how to set a password for your SAFE account.

In the menu mouse-over `login accounts` and click on `request login accounts`.

![Login Accounts menu]({{ "/splats/images/hpc1/login_accounts.png" | absolute_url }})

In the `projects` dropdown menu select `d411` about one third of the way down. Click `Next` and `Next` again on the following page.

![Login account request, project]({{ "/splats/images/hpc1/login_account_project.png" | absolute_url }})

Construct a username of your inital and surname (maximum length is 8 characters) and complete by clicking `Request`.

![Login account request, username]({{ "/splats/images/hpc1/login_account_username.png" | absolute_url }})

Your login account request is handled by a real person: [Dr. Gavin Pringle](https://www.epcc.ed.ac.uk/about/staff/dr-gavin-pringle).  Please be patient, this could take up to a day or two.

**Please note that you agree not to use the facilities for any nefarious purpose. This is a shared scientific resource and we are lucky to have access. No, you cannot mine cryptocurrency. Strictly only BIOC0023 data analysis!**

## Logging onto Cirrus

Once Dr. Pringle has approved your account, you will receive another email.

Log onto the [Tier 2 SAFE](https://www.archer.ac.uk/tier2/) again. When you mouse over `login accounts`, you should now see the username you picked next to `@Cirrus` listed in the menu.

Click on the link to get taken to your `Login accounts details`, which contains usage and budget statistics for your HPC use (more about this in term 2).

![Login account details]({{ "/splats/images/hpc1/login_account_details.png" | absolute_url }})

Click on `View Login Account Password` to see you login password. Select your password and copy it into your clipboard (right-click > copy or `Ctrl/Cmd+C`). No need to save it into your password-safe, as you will have to change it upon first login.

![Cirrus login password]({{ "/splats/images/hpc1/cirrus_login_password.png" | absolute_url }})

In order to log onto cirrus, start your terminal application of choice (e.g. Git Bash for Windows or Terminal.app on MacOS) and execute the following command. 

**All the example commands use my username - `mbaron`. Make sure to replace it with your username.**

```bash
ssh mbaron@login.cirrus.ac.uk
```

You will be asked to confirm the host authenticity by typing `yes` (and hitting enter). This will add `login.cirrus.ac.uk` to your lists of known hosts (in `~/.ssh/known_hosts`).

Following this, you will be prompted to enter the password you picked up from the Tier 2 SAFE. As we copied this into the clipboard, you can just paste it here. On a Mac, use `Cmd+V`, on the Windows GitBash this won't work; by default paste is set to right-click.

**Please note! The password cursor won't move. No matter how much you type or paste!**

```
[mbaron@partridge-wd00 ~]$ ssh mbaron@login.cirrus.ac.uk
The authenticity of host 'login.cirrus.ac.uk (129.215.175.28)' can't be established.
ECDSA key fingerprint is SHA256:EJp7y5JzeMseazlGb8qOQuGd52xw1SncA7m60G7axyI.
ECDSA key fingerprint is MD5:aa:a8:3c:4e:5d:75:ef:9f:f5:13:e7:41:95:1a:c8:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'login.cirrus.ac.uk,129.215.175.28' (ECDSA) to the list of known hosts.
mbaron@login.cirrus.ac.uk's password:
```

You will be greeted on Cirrus by a welcome message (and potentially other system update messages and warnings). You will also be immediately prompted to change your password:

First paste the same password again (note the prompt mentions `(current) UNIX password`). Then you'll be asked to enter you new password twice. You are free to chose any password, though I would recommend using a password safe to autogenerate a password and paste it, rather than type it.

**Again, please note that the cursor won't move for password entry!**

```
mbaron@login.cirrus.ac.uk's password:
You are required to change your password immediately (root enforced)
Last login: Wed Nov  7 11:43:16 2018 from dhcp179175.biochem.ucl.ac.uk
================================================================================

Cirrus HPC Service

--------------------------------------------------------------------------------
This is a private computing facility. Access to this system is limited to those
who have been granted access by the operating service provider on behalf of the
issuing authority and use is restricted to the purposes for which access was
granted. All access and usage are governed by the terms and conditions of access
agreed to by all registered users and are thus subject to the provisions of the
Computer Misuse Act, 1990 under which unauthorised use is a criminal offence.
--------------------------------------------------------------------------------

For help please contact the Cirrus helpdesk at:
support@cirrus.ac.uk

================================================================================
WARNING: Your password has expired.
You must change your password now and login again!
Changing password for user mbaron.
Changing password for mbaron.
(current) UNIX password:
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
Connection to login.cirrus.ac.uk closed.
[mbaron@partridge-wd00 ~]$
```

## Creating SSH keys

In order to avoid continuously having to type your password whenever you log onto remote servers (such as Cirrus), **secure shell (SSH) keys** can be added to the server for password-less authentication.

However, first you should check whether you already have keys present, by listing the contents of the hidden `.ssh` directory in your home directory.

```bash
ls -al ~/.ssh
```

If the output is a `No such file or directory` error, or if neither `id_rsa` or `id_rsa.pub` are present, you will have to create keys.

```
[mbaron@partridge-wd00 ~]$ ls -l ~/.ssh
ls: cannot access /home/mbaron/.ssh: No such file or directory
```

```
[mbaron@partridge-wd00 ~]$ ls -l ~/.ssh
total 8
-rw-------. 1 mbaron mbaron 747 Feb 13  2018 authorized_keys
-rw-r--r--. 1 mbaron mbaron 385 Nov  7 15:34 known_hosts
```

However, if `id_rsa` or `id_rsa.pub` **are** present (likely because you created them in the Git and GitHub workshop), skip to the next section.

You generate the keys using the `ssh-keygen` command. The flags `-t` sets the encryption algorithm, in this case RSA, and `-b` sets the key size, 4096 bits. For more information, have a look at the [ssh key-gen documentation](https://www.ssh.com/ssh/keygen/).

```bash
ssh-keygen -t rsa -b 4096
```

This will prompt a reply of `Generating public/private rsa key pair.` When you're prompted to `Enter a file in which to save the key (/Users/you/.ssh/id_rsa):` press `Enter` to accept the default location in `~/.ssh`.

Finally you are prompted to enter a passphrase. The official recommendation is to add a passphrase, in particular if you use your keys widely (people would still need to know this password to use your key). However, unless you are diligent about your password management, I would recommend leaving the passphrase empty.

If you want to use a passphrase, have a read on how to use an [SSH-agent to manage your passphrases for you](https://www.ssh.com/ssh/agent).

```
[mbaron@partridge-wd00 ~]$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/mbaron/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/mbaron/.ssh/id_rsa.
Your public key has been saved in /home/mbaron/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:o2icQTkSMX4qLXetWTNBhiTwskFeJF4NPsZd4bUbirQ mbaron@partridge-wd00.biochem.ucl.ac.uk
The key's randomart image is:
+---[RSA 4096]----+
|.+*B+.oo..       |
|+o*+o=o . .      |
|oo+==o.. o       |
| =.=ooo.. o      |
|+ + oE=.S.       |
| + o B + .       |
|    B .          |
|   .             |
|                 |
+----[SHA256]-----+
```

You will end up with two keys in your `~/.ssh/` folder.

```bash
ls -l ~/.ssh
```

`id_rsa.pub` is your public key, which you will share with the world, whilst `id_rsa` is your private key, which you should **not** share with anyone.

```
[mbaron@partridge-wd00 ~]$ ls -l ~/.ssh
total 16
-rw-------. 1 mbaron mbaron  747 Feb 13  2018 authorized_keys
-rw-------. 1 mbaron mbaron 3243 Nov  7 15:55 id_rsa
-rw-r--r--. 1 mbaron mbaron  765 Nov  7 15:55 id_rsa.pub
-rw-r--r--. 1 mbaron mbaron  385 Nov  7 15:34 known_hosts
```

## Copying your SSH keys to Cirrus

You can copy your keys to remote servers using the `ssh-copy-id` routine. Please note that the command below is for your **public key**!

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub mbaron@login.cirrus.ac.uk
```

You will be promoted to enter your password for Cirrus. This should be the last time on this machine. Again, cursors don't move for password entry on the command line.

**Please note that this needs to be the new password you created earlier, not the one from the SAFE!**

```
[mbaron@partridge-wd00 ~]$ ssh-copy-id -i ~/.ssh/id_rsa.pub mbaron@login.cirrus.ac.uk
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/mbaron/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
mbaron@login.cirrus.ac.uk's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'mbaron@login.cirrus.ac.uk'"
and check to make sure that only the key(s) you wanted were added.
```

If using `ssh-copy-id` returns a `command not found` error, you will have to resort to slightly more 'old-fashioned' methods to copy your public key onto cirrus.

```bash
cat ~/.ssh/id_rsa.pub | ssh mbaron@login.cirrus.ac.uk "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```

Once your keys are on Cirrus, you should be able to log on without having to enter your password and be greeted by Cirrus' welcome message.

```bash
ssh mbaron@login.cirrus.ac.uk
```

Notice that your command prompt (the text before your blinking cursor) changes, depending on the system you are currently on. In the example output below `[mbaron@partridge-wd00 ~]$` for my local desktop machine and `mbaron:~$` while on Cirrus - in both cases `~` indicates the user home directory.

Log off Cirrus again with the `exit` command.

```
[mbaron@partridge-wd00 ~]$ ssh mbaron@login.cirrus.ac.uk
Last failed login: Wed Nov  7 15:58:23 GMT 2018 from partridge-wd00.biochem.ucl.ac.uk on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Wed Nov  7 15:34:29 2018 from partridge-wd00.biochem.ucl.ac.uk
================================================================================

Cirrus HPC Service

--------------------------------------------------------------------------------
This is a private computing facility. Access to this system is limited to those
who have been granted access by the operating service provider on behalf of the
issuing authority and use is restricted to the purposes for which access was
granted. All access and usage are governed by the terms and conditions of access
agreed to by all registered users and are thus subject to the provisions of the
Computer Misuse Act, 1990 under which unauthorised use is a criminal offence.
--------------------------------------------------------------------------------

For help please contact the Cirrus helpdesk at:
support@cirrus.ac.uk

================================================================================


****  TMPDIR *must* be set if you are using Qiime1.           ****
****  Please contact g.pringle@epcc.ed.ac.uk for assistance.  ****


mbaron:~$
mbaron:~$ exit
logout
Connection to login.cirrus.ac.uk closed.
```

## Creating an SSH config

Typing all these ssh-commands can become arduous quickly. Let's make it easier, by creating an **SSH config** file. In the folder `~/.ssh/`, create a text-file called `config`. There are several ways to do this, but one of the fastest is to use the command line text-editor `vim`.

```bash
vim ~/.ssh/config
```

Using *vim* can be a bit counterintuitive at first. For basic use, you need to know that *vim* works with modes. By default it is in 'normal'/command mode. In order to type text, switch into the `insert` mode by hittin `i` and enter or paste (don't forget to edit) the content below (you can hit `tab` for the indents).

```vim
IdentityFile ~/.ssh/id_rsa
Host cirrus
	User mbaron
	Hostname login.cirrus.ac.uk
Host cirrus_data
	User mbaron
	Hostname dsn.cirrus.ac.uk
```

Once you have finished editing, hit `ESC` to return to command mode. In order to save your file type `:w`, you should see what you type in the bottom left corner of your window, and confirm by hitting `Enter`. To quit *vim* type `:q`, to quit without saving changes use `:q!`

Though seemingly tedious, *vim* is a handy editor to know, as it is available on virtually all unix-type systems.

Check the contents of the file you created using the concatenate (`cat`) command or the file reader `less` (to quit *less* hit `q`).

```bash
cat ~/.ssh/config
```

```
[mbaron@partridge-wd00 ~]$ vim ~/.ssh/config
[mbaron@partridge-wd00 ~]$ cat ~/.ssh/config
IdentityFile ~/.ssh/id_rsa
Host cirrus
	User mbaron
	Hostname login.cirrus.ac.uk
Host cirrus_data
	User mbaron
	Hostname dsn.cirrus.ac.uk
[mbaron@partridge-wd00 .ssh]$
```

You should now be able to connect to Cirrus using the much shorter command `ssh cirrus`.

If you receive a permission error, as below, update the file permissions using `chmod 600 ~/.ssh/config` and `chown $USER ~/.ssh/config`.

```
[mbaron@partridge-wd00 .ssh]$ ssh cirrus
Bad owner or permissions on /home/mbaron/.ssh/config
[mbaron@partridge-wd00 .ssh]$ chmod 600 ~/.ssh/config
```

## Installing QIIME on Cirrus

Before installing QIIME, we need to setup a local temporary directory. QIIME writes lots of temporary data, which could potentially break parts of the supercomputer. **Hence, this step is very important!** This is noted in every welcome message when you log onto Cirrus (see below).

```
****  TMPDIR *must* be set if you are using Qiime1.           ****
****  Please contact g.pringle@epcc.ed.ac.uk for assistance.  ****
```

Create a new directory using the `mkdir` command and use `export` to set it as a new environment variable. In order for this to happen every time you log on, we add this command to `.bashrc` (a script run every time a shell is started in interactive mode). We then load ('refresh') `.bashrc` in the current shell session with `source`.

```bash
mkdir ~/qiime_tmp
echo "export TMPDIR=~/qiime_tmp" >> ~/.bashrc
source ~/.bashrc
```

On Cirrus, you have access to a whole range of centrally installed software. For easy QIIME installation we will load *Miniconda* a Python distribution with a handy package manager called *conda* using the `module load` command. You can check which modules are loaded with the `module list` command and query available packages with `module avail`.

```bash
module load miniconda/python2
module list
```

Make sure to load `miniconda/python2` (not version 3).

```
mbaron:~$module avail miniconda

-------------------------------------- /lustre/sw/modulefiles --------------------------------------
miniconda/python2 miniconda/python3
mbaron:~$module load miniconda/python2
mbaron:~$module list
Currently Loaded Modulefiles:
  1) miniconda/python2
```

With *Miniconda* loaded, we can us its `conda` package manager to install QIIME with a single command. Best to copy & paste this. More information on QIIME installation can be found on the [developers' website](http://qiime.org/install/install.html).

The command will install a whole range of software-packages with precisely defined version in a virtual environment called `qiime1`. This prevents any previously installed software from breaking interactions between all the different packages.

```bash
conda create -n qiime1 python=2.7 qiime matplotlib=1.4.3 mock nose -c bioconda
```

After fetching packages (could take a minute or two) and informing you about which packages will be installed, you need to confirm the installation procedure with `y`.

```
mbaron:~$conda create -n qiime1 python=2.7 qiime matplotlib=1.4.3 mock nose -c bioconda
Fetching package metadata ...........
Solving package specifications: .
Warning: 4 possible package resolutions (only showing differing packages):
  - bioconda::qiime-1.9.1-np110py27_0, bioconda::scikit-bio-0.2.3-np110py27_0
  - bioconda::qiime-1.9.1-np110py27_0, bioconda::scikit-bio-0.2.3-py27_0
  - bioconda::qiime-1.9.1-py27_0, bioconda::scikit-bio-0.2.3-np110py27_0
  - bioconda::qiime-1.9.1-py27_0, bioconda::scikit-bio-0.2.3-py27_0

Package plan for installation in environment /lustre/home/d411/mbaron/.conda/envs/qiime1:

The following NEW packages will be INSTALLED:

    backports:               1.0-py27_0
    biom-format:             2.1.5-py27_4         bioconda
    blas:                    1.0-mkl

    *** many more packages, removed for brevity sake ***

    wcwidth:                 0.1.7-py27_0
    wheel:                   0.29.0-py27_0
    zlib:                    1.2.11-0

Proceed ([y]/n)? y
```

Now you can hang back and watch the installer download and install all the packages. This may take a minute or two.

```
blas-1.0-mkl.t 100% |####################################################| Time: 0:00:00   1.37 MB/s
libgfortran-3. 100% |####################################################| Time: 0:00:00   8.64 MB/s
libiconv-1.14- 100% |####################################################| Time: 0:00:00  20.07 MB/s

*** many more packages, removed for brevity sake ***

emperor-0.9.51 100% |####################################################| Time: 0:00:00  25.91 MB/s
pynast-1.2.2-p 100% |####################################################| Time: 0:00:00  11.52 MB/s
qiime-1.9.1-np 100% |####################################################| Time: 0:00:01   9.59 MB/s
#
# To activate this environment, use:
# > source activate qiime1
#
# To deactivate this environment, use:
# > source deactivate qiime1
#
```

Once the installation has completed, we need to check it is working. First the `qiime1` environment has to be activated and we can then execute a [QIIME function](http://qiime.org/scripts/print_qiime_config.html), which runs a few basic tests.

```bash
source activate qiime1
print_qiime_config.py -t
```

You should see the command prompt change, with the environment name (here `qiime1`) prepended in parentheses. The script will output the versions of software dependencies, as well as other QIIME configuration information (useful for trouble shooting). If the installation was successful, you should pass all 9 tests.

```
mbaron:~$source activate qiime1
(qiime1) mbaron:~$print_qiime_config.py -t

System information
==================
         Platform:	linux2
   Python version:	2.7.13 |Continuum Analytics, Inc.| (default, Dec 20 2016, 23:09:15)  [GCC 4.4.7 20120313 (Red Hat 4.4.7-1)]
Python executable:	/lustre/home/d411/mbaron/.conda/envs/qiime1/bin/python

QIIME default reference information
===================================
For details on what files are used as QIIME's default references, see here:
 https://github.com/biocore/qiime-default-reference/releases/tag/0.1.3

Dependency versions
===================
          QIIME library version:	1.9.1
           QIIME script version:	1.9.1
qiime-default-reference version:	0.1.3
                  NumPy version:	1.10.4
                  SciPy version:	0.17.1
                 pandas version:	0.18.1
             matplotlib version:	1.4.3
            biom-format version:	2.1.5
                   h5py version:	2.6.0 (HDF5 version: 1.8.16)
                   qcli version:	0.1.1
                   pyqi version:	0.3.2
             scikit-bio version:	0.2.3
                 PyNAST version:	1.2.2
                Emperor version:	0.9.51
                burrito version:	0.9.1
       burrito-fillings version:	0.1.1
              sortmerna version:	SortMeRNA version 2.0, 29/11/2014
              sumaclust version:	SUMACLUST Version 1.0.00
                  swarm version:	Swarm 1.2.19 [Mar  5 2016 16:56:02]
                          gdata:	Installed.

QIIME config values
===================
For definitions of these settings and to learn how to configure QIIME, see here:
 http://qiime.org/install/qiime_config.html
 http://qiime.org/tutorials/parallel_qiime.html

                     blastmat_dir:	None
      pick_otus_reference_seqs_fp:	/lustre/home/d411/mbaron/.conda/envs/qiime1/lib/python2.7/site-packages/qiime_default_reference/gg_13_8_otus/rep_set/97_otus.fasta
                         sc_queue:	all.q
      topiaryexplorer_project_dir:	None
     pynast_template_alignment_fp:	/lustre/home/d411/mbaron/.conda/envs/qiime1/lib/python2.7/site-packages/qiime_default_reference/gg_13_8_otus/rep_set_aligned/85_otus.pynast.fasta
                  cluster_jobs_fp:	start_parallel_jobs.py
pynast_template_alignment_blastdb:	None
assign_taxonomy_reference_seqs_fp:	/lustre/home/d411/mbaron/.conda/envs/qiime1/lib/python2.7/site-packages/qiime_default_reference/gg_13_8_otus/rep_set/97_otus.fasta
                     torque_queue:	friendlyq
                    jobs_to_start:	1
                       slurm_time:	None
            denoiser_min_per_core:	50
assign_taxonomy_id_to_taxonomy_fp:	/lustre/home/d411/mbaron/.conda/envs/qiime1/lib/python2.7/site-packages/qiime_default_reference/gg_13_8_otus/taxonomy/97_otu_taxonomy.txt
                         temp_dir:	/lustre/home/d411/mbaron/qiime_tmp/
                     slurm_memory:	None
                      slurm_queue:	None
                      blastall_fp:	blastall
                 seconds_to_sleep:	1

QIIME base install test results
===============================
.........
----------------------------------------------------------------------
Ran 9 tests in 0.028s

OK
```

Finally, as Cirrus is a high-performance compute cluster, it doesn't have a display and hence no need for a window manager. Part of QIIME, `matplotlib` needs to be informed and switch to a mode, which doesn't expect a display. To following code will add this configuration.

```bash
echo "backend: Agg" > ~/.config/matplotlib/matplotlibrc
```

Lastly, deactivate the `qiime1` environment and log off Cirrus.

```bash
source deactivate
exit
```

You should see the command prompt change twice, once when the environment is closed and once more when you log off Cirrus.

```
(qiime1) mbaron:~$source deactivate
mbaron:~$exit
logout
Connection to login.cirrus.ac.uk closed.
[mbaron@partridge-wd00 .ssh]$
```

## Cirrus user documentation

You can find more information about Cirrus in its [user documentation](https://cirrus.readthedocs.io/en/latest/).