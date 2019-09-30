---
layout: splat
title: Accessing the Supercomputer (Cartesius) and installing QIIME2
---

You will get access to the Dutch national supercomputer [SURFsara Cartesius](https://userinfo.surfsara.nl/systems/cartesius). This guide will walk you through account setup, setup of ssh-keys and software installation.

## Cartesius user account

You will receive an email (likely by *helpdesk@surfsara.nl* or a similar SURFsara address) providing you with information on how to activate your account. Follow the instructions.

If you have not received an account by the end of the first week of term, please get in touch with your course organiser.

<!-- Fill in the [this Google form](https://docs.google.com/forms/d/e/1FAIpQLSdqfPfXoVtL20lTIFC5iB04RPbujXPiVx7wOG_fUSywBuydQQ/viewform). All parts of the form are required, which means you should read through the user agreement, which is linked. **You do not need to fill in the attached word-document.**

Please use your full name and **UCL email address**!

![SurfSARA user agreement form](surfsara_google_form.png)

Please be patient. These account requests are handled by [Dr Marco Verdicchio](https://www.linkedin.com/in/marco-verdicchio/), who, while a stellar supercomputing consultant, is also very much human.

Once your user agreement data has been processed, you will receive an email detailing your login-name and an initial password. Follow the email's instructions to update your password on the SURFsara user portal.

![new login username created at SURFsara](surfsara_password.png) -->

> **Please note:** Cartesius is to be used for BIOC0023 data analysis only! This is a shared scientific resource and we are very privileged to have access. You most definitely must not mine cryptocurrency or pursue other nefarious applications.

## Logging onto Cartesius via the SURFsara doornode

Only connections from within the SURFsara network, or whitelisted IP-addresses can connect to Cartesius directly. The **SURFsara doornode** enables an indirect connection through the SURFsara network.

Start your terminal application of choice (e.g. Git Bash for Windows or Terminal.app on MacOS) and execute the following command. 

*Note: All the example commands use my username - `mbaron`. Make sure to replace it with your username.*

```bash
ssh mbaron@doornode.surfsara.nl
```

You will be asked to confirm the host authenticity by typing `yes` (and hitting enter). This will add `doornode.surfsara.nl` to your lists of known hosts.

```example_output
$ ssh mbaron@doornode.cartesius.nl
The authenticity of host 'doornode.cartesius.nl (129.215.175.28)' can't be established.
ECDSA key fingerprint is SHA256:EJp7y5JzeMseazlGb8qOQuGd52xw1SncA7m60G7axyI.
ECDSA key fingerprint is MD5:aa:a8:3c:4e:5d:75:ef:9f:f5:13:e7:41:95:1a:c8:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'doornode.cartesius.nl,129.215.175.28' (ECDSA) to the list of known hosts.
```

You will be prompted to enter your updated password. **The cursor does not move during password entry!** No dots will appear; no feedback whatsoever. Either type very deliberately, or, even better, copy and paste the password.

```example_output
mbaron at dhcp179175 in ~$ ssh mbaron@doornode.surfsara.nl
                              SURFsara

                        Welcome to SURFsara

   This is a private computer facility.   Access for any reason must be
   specifically authorized by the owner.  Unless you are so authorized,
   your continued  access and any other use may  expose you to criminal
   and/or civil proceedings.

   Information:          http://www.surfsara.nl

mbaron@doornode.surfsara.nl's password:
```

Select `cartesius` from the menu. You will be asked to type in your password one more time.

```example_output
mbaron@cartesius.surfsara.nl's password:
Last login: Wed Jul 31 09:55:07 2019 from socrates-a-14-03-375-d04.isd-nat.ucl.ac.uk
********************************************************************************
* Questions? Please e-mail to helpdesk@surfsara.nl, or call 020-8001400        *
******************************************* last modified:  01-08-2019 16:00 ***
[mbaron@int1 ~]$
```

Congratulations, you have successfully conntected to Cartesius. Notice that your command prompt (the text before your blinking cursor) changes, depending on the system you are currently on. In the example outputs above `mbaron at dhcp179175 in ~$` for my local desktop machine changes to `[mbaron@int1 ~]$` while on Cartesius - in both cases `~` indicates the user home directory.

Let's disconnect by either execulting the `logout` or `exit` command or the key-combination `Ctrl+D`.

## Logging onto Cartesius via Socrates

Private IP addresses change ever so often and requesting lots of white-listed IP addresses is impractical. Thus we will use our own "doornode" - **UCL's Socrates**.

Socrates is a server already setup to access UCL reserach resources from outside the UCL network. Its IP has also been whitelisted with Cartesius (and it won't change).

We will configure **secure shell (SSH)** keys for password-free access and some clever SSH routing to default access through UCL Socrates.

### Creating SSH keys

*Note: Make sure you are not connected to Cartesius any more! Check your command prompt.*

In order to avoid continuously having to type your password whenever you log onto remote servers, **secure shell (SSH) keys** can be added to the server for password-less authentication.

However, first you should check whether you already have keys present, by listing the contents of the hidden `.ssh` directory in your home directory.

```bash
ls -al ~/.ssh
```

If the output is a `No such file or directory` error, or if neither `id_rsa` or `id_rsa.pub` are present, you will have to create keys.

```example_output
$ ls -l ~/.ssh
ls: cannot access /home/mbaron/.ssh: No such file or directory
```

```example_output
$ ls -l ~/.ssh
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

```example_output
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

```example_output
$ ls -l ~/.ssh
total 16
-rw-------. 1 mbaron mbaron  747 Feb 13  2018 authorized_keys
-rw-------. 1 mbaron mbaron 3243 Nov  7 15:55 id_rsa
-rw-r--r--. 1 mbaron mbaron  765 Nov  7 15:55 id_rsa.pub
-rw-r--r--. 1 mbaron mbaron  385 Nov  7 15:34 known_hosts
```

### Copying your public SSH keys to Socrates

You can copy your keys to remote servers using the `ssh-copy-id` routine. Please note that the command below is for your **public key**!

*Note: Replace `username` with your UCL username (e.g. `zcbcaxx`) in the example commands below.*

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub username@socrates.ucl.ac.uk
```

You will be asked to confirm the host authenticity by typing `yes` (and hitting enter). When prompted enter your UCL password. **Note that the cursor won't move when you type!**

A successful transfer will be will be confirmed with an output similar to the one below.

```example_output
$ ssh-copy-id -i ~/.ssh/id_rsa.pub zcbcaxx@socrates.ucl.ac.uk
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/mbaron/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
zcbcaxx@socrates.ucl.ac.uk's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'zcbcaxx@socrates.ucl.ac.uk'"
and check to make sure that only the key(s) you wanted were added.
```

If using `ssh-copy-id` returns a `command not found` error, you will have to resort to slightly more 'old-fashioned' methods to copy your public key onto Socrates.

```bash
cat ~/.ssh/id_rsa.pub | ssh username@socrates.ucl.ac.uk "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && cat >>  ~/.ssh/authorized_keys"
```

Once your keys are on Socrates, you should be able to log on without having to enter your password and be greeted by Socrates' welcome message.

```bash
ssh username@socrates.ucl.ac.uk
```
Again, notice the command prompt change. On Socrates you should see a number followed by the percentage sign, e.g. `25 %`.

```example_output
mbaron at dhcp179175 in ~
$ ssh username@socrates.ucl.ac.uk

 ------------------------------------------------------------------------
 Information Systems - Information Services Division - UCL

 Access to and use of this system are restricted to authorised individuals
 and subject to UCL Computing Regulations.
 ------------------------------------------------------------------------

Last login: Tue Aug 20 15:46:00 2019 from dhcp179175.biochem.ucl.ac.uk

------------------------------------------------------------------------
---                This machine is managed by Puppet 4               ---
------------------------------------------------------------------------

Information Services Division - UCL

Access to and use of this system are restricted to authorised individuals
and subject to UCL Computing Regulations.

All IS systems run unattended overnight and at weekends.
If they fail services may not be restored until the next working day.

ISD Service Desk Hours

   Monday to Friday -  08.30 - 17.30  (Telephone Support)
   Monday to Friday -  09.30 - 17.00  (Personal Callers)

   Vacation times will vary

For ISD Service Desk contact information, see www.ucl.ac.uk/isd/servicedesk
For Service News, see www.ucl.ac.uk/isd/news

Please report any service issues to the ISD Service Desk.

PLEASE NOTE- Files in the /tmp file system are deleted at regular intervals.
Please ensure that any important files are stored in your home directory.
-----------------------------

25 %
```

Log off Socrates again with the `logout` command.

### Setup of SSH configuration and routing

*Note: Make sure you are not connected to Socrates! Check the command prompt.*

In order to one-step connect to Cartesius via Socrates, we will apply a little routing 'magic'. For this, we will create/edit an **SSH config**. In the folder `~/.ssh/`, create a text-file called `config`. There are several ways to do this, but one of the fastest is to use the command line text-editor `vim`.

```bash
vim ~/.ssh/config
```

Using *vim* can be a bit counterintuitive at first. For basic use, you need to know that *vim* works with modes. By default it is in 'normal'/command mode. In order to type text, switch into the `insert` mode by hittin `i` and enter or paste (don't forget to edit) the content below (you can hit `tab` for the indents). Make sure you replace `cartesius_username` and `ucl_username` appropriately.

```ssh
IdentityFile ~/.ssh/id_rsa
Host socrates
      User ucl_username
      Hostname socrates.ucl.ac.uk
Host cartesius
      User cartesius_username
      Hostname cartesius.surfsara.nl
      proxyCommand ssh -W cartesius.surfsara.nl:22 socrates
```

Once you have finished editing, hit `ESC` to return to command mode. In order to save your file type `:w`, you should see what you type in the bottom left corner of your window, and confirm by hitting `Enter`. To quit *vim* type `:q`, to quit without saving changes use `:q!`

Though seemingly tedious, *vim* is a handy editor to know, as it is available on virtually all unix-type systems.

Check the contents of the file you created using the concatenate (`cat`) command or the file reader `less` (to quit *less* hit `q`).

```example_output
mbaron at dhcp179175 in ~
$ cat .ssh/config
IdentityFile ~/.ssh/id_rsa
Host socrates
      User zcbcaxx
      Hostname socrates.ucl.ac.uk
Host cartesius
      User mbaron
      Hostname cartesius.surfsara.nl
      proxyCommand ssh -W cartesius.surfsara.nl:22 socrates
```

Now you should be able to connect to Cartesius directly. Any connection will be routed through UCL's Socrates server and hence appear is if originated from there (and its whitelisted IP). Note that you won't have to type your username anymore, as this is defined in the ssh-config.

```bash
ssh cartesius
```

Should you receive a `permission denied` error, execute the two commands below and then try again.

```bash
# changes the files owner to the current user
chown $USER ~/.ssh/config
# changes the file permissions read/write for file-owner and read for all others users
chmod 644 ~/.ssh/config
```

You will still be asked to enter your password on Cartesius. To avoid this you need to upload your public key to Cartesius as well.

### Copying your public SSH keys to Cartesius

As previously we can use `ssh-copy-id`. Note that you won't have to add any usernames this time, as they are defined in your ssh-config.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub cartesius
```

Should `ssh-copy-id` return an error, you can use the more 'old-fashioned' way below.

```bash
cat ~/.ssh/id_rsa.pub | ssh cartesius "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && cat >>  ~/.ssh/authorized_keys"
```

You should now be able to connect to Cartesius with a simple `ssh cartesius` command. You should see the welcome messages of Socrates and Cartesius, as well as the bash-prompt change.

```example_output
mbaron at dhcp179175 in ~
$ ssh cartesius

 ------------------------------------------------------------------------
 Information Systems - Information Services Division - UCL

 Access to and use of this system are restricted to authorised individuals
 and subject to UCL Computing Regulations.
 ------------------------------------------------------------------------

Last login: Tue Aug 20 16:48:31 2019 from doornode.osd.surfsara.nl
********************************************************************************
* Questions? Please e-mail to helpdesk@surfsara.nl, or call 020-8001400        *
******************************************* last modified:  01-08-2019 16:00 ***
[mbaron@int2 ~]$
```

### Copying files to and from Cartesius

You can now connect and copy files to (or from) Cartesius with single commands without having to add your username or type your password.

*Note: make sure you are not connected to Cartesius (or Socrates)! You need to be on your local PC. Check your command prompt.*

To test this, let's copy a dummy file to your home directory (`~`) on Cartesius using secure-copy (`scp`). 

In the example below, `cd` first takes you back to your user's home directory. `touch` creates an empty file (it usually updates the timestamp, but can be 'abused' to make new files).The file is then transfered from the local home directory to the home directory (`~`) on Cartesius. Note that the destination path on Cirrus follows after the colon (`:`).

```bash
# moves you to your home directory
cd
# creates an empty file
touch dummy.file
# scp [source] [destination]
scp dummy.file cartesius:~
```

```expected_output
mbaron at dhcp179175 in ~$ cd
mbaron at dhcp179175 in ~$ touch dummy.file
mbaron at dhcp179175 in ~$ scp dummy.file cartesius:~
dummy.file
```

Connect to Cartesius and list the contents of your home directory. You should see the dummy file.

```bash
ssh cartesius
# welcome message will appear, bash-prompt will change
ls -l
```

Let's create a dummy file on Cartesius, log off Cartesius and copy this back to your PC.

```bash
# this happens on Cartesius
touch dummy2.file
exit
# after exit you should be on your PC again (notice the bash-prompt change)
scp cartesius:~/dummy2.file ~
ls -l
```

As you can see the format for `scp` is always source(s) to destination (`.` being the current directory). Because your local machine is not setup to be addressed **from** Cartesius, you always have to initiate file transfers from your local machine.

## Installing QIIME2 on Cartesius

*Note: Make sure you are connected to Cartesius! Check your command prompt.*

QIIME2 is a collection of software tools bundled together and connected by Python code. Both for the installation and in order to execute QIIME Python is needed.

Luckily, supercomputers come with a host of programmes, which can be loaded as required with the `module` commands. You can find more information in the [Cartesius documentation on modules](https://userinfo.surfsara.nl/systems/shared/modules).

Cartesius uses [EasyBuild](https://easybuilders.github.io/easybuild/), a software build and installation framework, which needs to be loaded first. This is followed by a load command for [Anaconda](https://www.anaconda.com/), a comprehensive Python distribution with the handy `conda` Python package manager.

```bash
module load eb
module load Anaconda3
```

If you compare the packages used for Python before and after module loading, you should see the paths and versions change.

```example_output
[mbaron@int1 ~]$ which python
/usr/bin/python
[mbaron@int1 ~]$ python --version
Python 2.7.5
[mbaron@int1 ~]$ module load eb
[mbaron@int1 ~]$ module load Anaconda3
[mbaron@int1 ~]$ which python
/hpc/eb/RedHatEnterpriseServer7/Anaconda3/5.0.1/bin/python
[mbaron@int1 ~]$ python --version
Python 3.6.3 :: Anaconda, Inc.
```

The `conda` package manager makes it possible to install QIIME with only a few commands. More information on QIIME installation can be found on the [developers' website](https://docs.qiime2.org/2019.7/install/native/#install-qiime-2-within-a-conda-environment).

The command will install a whole range of software-packages with precisely defined version in a virtual environment called `qiime2-2019.7` (latest version at the time of writing). A virtual environment prevents any previously installed software from breaking interactions between all the different packages; it helps to keep the dependencies required by all the software packages.

First download the environment file with `wget`.

```bash
wget https://data.qiime2.org/distro/core/qiime2-2019.7-py36-linux-conda.yml
```
```example_output
[mbaron@int1 ~]$ wget https://data.qiime2.org/distro/core/qiime2-2019.7-py36-linux-conda.yml
--2019-08-21 14:58:17--  https://data.qiime2.org/distro/core/qiime2-2019.7-py36-linux-conda.yml
Resolving data.qiime2.org (data.qiime2.org)... 52.35.38.247
Connecting to data.qiime2.org (data.qiime2.org)|52.35.38.247|:443... connected.
HTTP request sent, awaiting response... 302 FOUND
Location: https://raw.githubusercontent.com/qiime2/environment-files/master/2019.7/release/qiime2-2019.7-py36-linux-conda.yml [following]
--2019-08-21 14:58:17--  https://raw.githubusercontent.com/qiime2/environment-files/master/2019.7/release/qiime2-2019.7-py36-linux-conda.yml
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.36.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.36.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6287 (6.1K) [text/plain]
Saving to: ‘qiime2-2019.7-py36-linux-conda.yml’

100%[===============================================================================================================>] 6,287       --.-K/s   in 0s

2019-08-21 14:58:18 (84.5 MB/s) - ‘qiime2-2019.7-py36-linux-conda.yml’ saved [6287/6287]
```

Then install the software in a virtual environment through `conda`.

```bash
conda env create -n qiime2-2019.7 --file qiime2-2019.7-py36-linux-conda.yml
```

Installation might appear stuck after all the downloads have passed, however, remain patient as this may take a couple of minutes (4.3GB worth of software are installed).

```example_output
[mbaron@int1 ~]$ conda env create -n qiime2-2019.7 --file qiime2-2019.7-py36-linux-conda.yml
Using Anaconda API: https://api.anaconda.org
Fetching package metadata .................
Solving package specifications: .
_r-mutex-1.0.1 100% |#########################################################################################################| Time: 0:00:00   1.76 MB/s

# ** maaaany lines removed **

q2-dada2-2019. 100% |#########################################################################################################| Time: 0:00:00   4.79 MB/s

Enabling notebook extension jupyter-js-widgets/extension...
      - Validating: OK

#
# To activate this environment, use:
# > source activate qiime2-2019.7
#
# To deactivate an active environment, use:
# > source deactivate
#
```

Once the installation has completed, we need to check whether it is working. First the `qiime2-2019.7` environment has to be activated. 

```bash
source activate qiime2-2019.7
```

You should see your command prompt change, with the environment name prepended in brackets. QIIME might carry out some housekeeping first as well.

```example_output
[mbaron@int1 ~]$ source activate qiime2-2019.7
QIIME is caching your current deployment for improved performance. This may take a few moments and should only happen once per deployment.
(qiime2-2019.7) [mbaron@int1 ~]$
```

Finally, the installation can be tested by running a QIIME function, such as `qiime info` or `qiime --help`.

```bash
qiime info
```

```example_output
(qiime2-2019.7) [mbaron@int1 ~]$ qiime info
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

Should you have come across an error, remove the environment and attempt the installation again.

```bash
# IGNORE THIS IF YOU HAD NO ERRORS!
# to remove the conda environment.
source deactivate
conda remove --name qiime2-2019.7 --all
```

With the environment active, install Jupyter lab.

```bash
conda install jupyterlab -y
```
```example_output
(qiime2-2019.7) [mbaron@int1 ~]$ conda install jupyterlab -y
Fetching package metadata ...........
Solving package specifications: .

Package plan for installation in environment /home/mbaron/.conda/envs/qiime2-2019.7:

The following NEW packages will be INSTALLED:

    json5:             0.8.5-py_0
    jupyterlab:        1.0.2-py36hf63ae98_0
    jupyterlab_server: 1.0.0-py_1

json5-0.8.5-py 100% |#########################################################################################################| Time: 0:00:00   3.40 MB/s
jupyterlab_ser 100% |#########################################################################################################| Time: 0:00:00   8.25 MB/s
jupyterlab-1.0 100% |#########################################################################################################| Time: 0:00:00  22.24 MB/s
```

As the last step, enable the QIIME2 extension for Jupyter.

```bash
jupyter serverextension enable --py qiime2 --sys-prefix
```
```expected_output
(qiime2-2019.7) [mbaron@int1 ~]$ jupyter serverextension enable --py qiime2 --sys-prefix
Enabling: qiime2.jupyter
- Writing config: /home/mbaron/.conda/envs/qiime2-2019.7/etc/jupyter
    - Validating...
      qiime2.jupyter  OK
```

Lastly, deactivate the `qiime2-2019.7` environment and log off Cartesius.

```bash
source deactivate
exit
```

Well done. You are all setup.

## Working on several computers

If you work on several machines, e.g. the UCL cluster room and your laptop, you may want to go through the setup of ssh-keys and the ssh-configuration on all your regularly used machines. You won't have to install QIIME on the supercomputer again.