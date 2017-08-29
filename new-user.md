# New User Elephant Exercise

## Logging In
* Windows - [Putty](https://the.earth.li/%7Esgtatham/putty/latest/x86/putty.exe "Putty")
* Mac/Linux - Use Terminal application

## Moving Around in the Environment
Things to look out for in a terminal:
* Your mouse does not work.
	* You have to use left and right arrows to change move the cursor
* Copy and paste work differently
* Your cursor will not move when you type in a password
* The prompt is the computer's way of letting you know it is ready for the next command

Things that will help you in a terminal:
* You can use the up arrow key back through a list of previous commands
* Tab completion is your friend!

First, I might want to know who I am.

```bash
$ whoami
nelle
```

Next, I might want to know where I am.

```bash
$ pwd
/home/nelle
```

`pwd` stands for print working directory and gives us our "address" on the computer.

We can change our location by using the "change directory" command.

```bash
$ cd /opt/example_submission_scripts/
$ pwd
/opt/example_submission_scripts
```

Notice that each of these file paths starts with `/`. This is called "root." You can think of the file system like a tree with all of the folders branching out from the root.

Recall that Cowboy has three major directories: `/home`, `/scratch`, and `/opt`. Notice we started in our `/home/nelle` folder and have now moved to a folder called `example_submission_scripts` inside of `/opt`.

We can see that `/` has two uses. Not only does it designate root, but it also serves as a divider between folder names.

> Every user has a `/scratch/nelle` folder as well. How do we use `cd` to move into this folder?

```bash
$ cd /scratch/nelle
$ pwd
/scratch/nelle
```

"Addresses" that start with `/` are known as absolute paths. You can use an *absolute path* to get from any place on the computer to any other place.

We have a shortcut for jumping back to the folder where we were just at.

```bash
$ cd -
/opt/example_submission_scripts
$ pwd
/opt/example_submission_scripts
```

Enough jumping around. How do we see the files that are in the folder we are located in? We can use the "list" command.

```bash
$ ls
blast+                 helloworld   matlab   python  README.md
compiling_and_running  mathematica  mrbayes  r
```

This directory contains several examples for new users to play with when learning how to use Cowboy. Today we will play with the python example. Let's check it out.

```bash
$ cd python
$ ls
elephant.pbs  elephant.py  README.md
```

There are three files within `python`. We will look at each one in a little bit. Notice that we can use `cd` with a *relative path* that does not start with `/`. This is analogous to telling a guest that you home is three blocks south of Main street on Elm.

Let's use a shortcut to go back to our home directory.

```bash
$ cd
$ pwd
/home/nelle
```

> What files do we have in our home directory?

```bash
$ ls
```

Right now we have nothing. Let's change that by making a directory.

```bash
$ mkdir my_folder
$ ls
my_folder
```

We can change the way that a program works by setting a "flag." This is like a switch that changes the default behavior of the program or command that is being called.

```bash
$ ls -a
.              .cache       .ghc        .matlab      .ssh
..             .config      .gitconfig  .matplotlib  .texlive2007
.bash_history  .cshrc       .gm_key     .mozilla     .vim
.bash_logout   .emacs       .gnome2     .nv          .viminfo
.bash_profile  .fontconfig  .history    .parallel    .Xauthority
.bashrc        .forward     .lesshst    .pki
.cabal         .gconf       .lmod.d     .sage
```

Suddenly our home folder has extra files! `-a` stands for all and it tells `ls` to list all the files in the directory including the hidden files. All files that start with `.` are hidden.

Two "files" here are important. They're actually not files but abbreviations. `.` stand for the current directory. `..` stands for the parent directory.

> Try to move up to the parent directory using `cd`.

```bash
$ cd ..
$ pwd
/home
```

> How do I get back home?

```bash
$ cd
$ pwd
/home/nelle
```

The "copy" command allows us to make a copy of a folder.

```bash
$ cp /opt/example_submission_scripts/python/ .
cp: omitting directory `/opt/example_submission_scripts/python/'
```

Why did we get an error? It turns out copy is designed to copy only single files. It can't move entire directories. We can change this behavior with the `--recursive` flag. Use the up arrow to go back to the last command and add `-r`.

```bash
$ cp -r /opt/example_submission_scripts/python/ .
$ ls
my_folder python
```

Let's remove our empty folder using the "remove" command.

```bash
$ rm my_folder
error:
```

It turns out `rm` is also designed for single files, but it also has a "recursive" flag.

```bash
$ rm -r my_folder
$ ls
python
```

**Warning!** There is no recycle bin ina Unix shell environment. Once a file is gone, it's gone forever!

The "move" function wears two hats. It cat be used to both move and rename files.

```bash
$ mv python my_python
$ ls
my_python
```

There are three files within the `python` directory.

```bash
$ cd my_python/
$ ls
elephant.pbs  elephant.py  README.md
```

It makes sense to start with the `README.md` file. One way we can look inside of files is by using `cat`.

```bash
$ cat README.md 
Python website: https://www.python.org/

These instructions assume you have already gone through the HPC new user tutorial.

To run this example on Cowboy:
       .
       .
       .
```

Most software will have a `README` file. Always start by reading its contents. If the file is too long, we can also use `less`. Let's use `less` to look at the Python code.

```bash
$ less elephant.py
```

We can scroll up and down in less by using the `j` and `k` keys. We can also use the space bar and `b` button to go up and down in a file one whole page at a time. We will not be worrying about the details of the Python code today, so let's quit out of `less`. To quit, type `q`.

The last file we have not looked at yet is `elephant.pbs`. This is the submit script. Since Cowboy is a shared resource, all jobs must be submitted to a scheduler. The scheduler then determines when and where your job will run. (Talk about the typical workflow on Cowboy and show the picture from the PPT). In this way, resources are shared equitably.

`elphant.pbs` is a script file that gives the scheduler the information it needs to run your program or code. We want to edit some of the things found in this code. We will use the text editor `nano`.

```bash
$ nano elephant.pbs
```

There are several parts to this file. At the bottom, we see

```bash
cd $PBS_O_WORKDIR
module load python/3.5.0

python elephant.py
```

This set of commands tells Cowboy to move into your home directory and run your python script once it is your turn. This section will only run once it's time for the job to start.

The lines in the first section all start with `#PBS`. This tells the scheduler that that line has information for the it about your job.

```bash
#PBS -q batch
```

This line tells the scheduler which queue you want your job to be in. Let's change to the express queue: `#PBS -q express`.

```bash
#PBS -l nodes=1:ppn=1
#PBS -l walltime=10:00
```

These two lines tell the scheduler that if your job is still running after ten minutes, that the scheduler can kill it, and that you are requesting one node and one processor on Cowboy.

Don't worry too much about the last line in this section. It is important that all your submission scripts have this line so that HPCC staff have access to all the information they need in case they need to help you troubleshoot your job.

Let's add a line to our submission script so that Cowboy will email us when our job has completed.

```bash
#PBS -m	abe -M your_email@okstate.edu
```

When you are done, use CTRL-X to exit out of nano. Make sure you type `y` and enter to save the changes you made to the file.

It's now time to submit your job.

```bash
$ qsub elephant.pbs 
824139.mgmt1
```

You can check on the status of your job.

```bash
$ showq -u nelle
ACTIVE JOBS--------------------
JOBNAME            USERNAME      STATE  PROC   REMAINING            STARTTIME

824139              pdoehle    Running     1    00:10:00  Tue Aug 29 11:41:53

     1 Active Job     2648 of 3264 Processors Active (81.13%)
                       224 of  262 Nodes Active      (85.50%)

IDLE JOBS----------------------
JOBNAME            USERNAME      STATE  PROC     WCLIMIT            QUEUETIME


0 Idle Jobs

BLOCKED JOBS----------------
JOBNAME            USERNAME      STATE  PROC     WCLIMIT            QUEUETIME


Total Jobs: 1   Active Jobs: 1   Idle Jobs: 0   Blocked Jobs: 0
```

It usually takes the scheduler about 30 seconds to update so you may have to wait a bit before you see the status of your job.

Let's see the results.

```bash
$ ls
elephant.pbs  elephant.pbs.o824139  elephant.png  elephant.py  README.md
```

There are two new files: `elephant.pbs.o824139`, and `elephant.png`. The file ending with your job number gives you a report of how the job went, and it just so happens that this script creates a picture of an elephant.

Let's look at the report.

```bash
$ cat elephant.pbs.o824139 
Saving figure
Done
```

This is what would normally show up on your terminal if you ran the program dynamically. Cowboy captures that output and saves it to a file for you.

Since we can't view a picture file while logged into Cowboy, we will need to use [Cyberduck](https://cyberduck.io/ "Cyberduck") to transfer the file to our local computer and view it there.

> Have students download Cyberduck and transfer the file to their computer to look at.

If you have any questions, please don't hesitate to contact us at hpcc@okstate.edu!

