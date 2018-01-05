# The Unix Shell
## Setup

- Download the [data](http://swcarpentry.github.io/shell-novice/data/shell-novice-data.zip) for today's lesson and move it to your desktop.

```bash
$ PS1='$ '
cd
```

## Background
- At a high level, computers do four things:
	- Run programs
	- Store data
	- Communicate with each other
	- Interact with us

- Interacting with computers
	- Prior to 1950's people interacted with computers by rewiring them
	- Since the 1980's, we have used icons windows, pointers, and similar technologies (GUI - graphical user interface)
	- Today speech recognition like Alexa or Google Home are becoming popular
	- Between 1950's and 1980's, most people used line printers

- Line printers are limited to allowing input and output with letters, numbers, and punctuation found on a standard keyboard.
	- This kind of interface is known as a *command-line interface* (CLI)
	- This interface works on a read-evaluate-print loop.
	- User types a command and presses `Enter`.
	- The computer reads, executes, and prints output
	- User types a new command.

- A shell is program that run on your computer and gives you a CLI to interact with the computer.

- The shell primarily calls other programs on the computer, but gives you a way to interact with those programs.

- Most modern Unix systems use the Bash shell by default.

- Why bother?
	- With a few keystrokes, we can combine tools to create powerful workflows.
	- It's easy to reproduce work that we have done before.
	- It's often the easiest way to connect to a remote computer, including supercomputers.

- Nelle's Pipeline
	- Nelle Nemo is a marine biologist who has just returned from the six-month survey of the North Pacific Gyre
	- She has collected 1,520 samples and needs to ...
		- Run each sample through an assay machine that will measure the relative abundance of 300 different proteins
		- Calculate statistics for each protein separately using a program called `goostats`
		- Compare statistics using a program called `goodiffi`
		- Write up the results for the upcoming issue of *Aquatic Goo Letters*, which is due at the end of the month.
	- It takes half an hour for the assay machine to process each sample, and two minutes to set each one up.
		- With 8 assay machines in the lab, this step will *only* take two weeks.
	- If she runs `goostats` and `goodiff` by hand, she will need to enter filenames and click `OK` 46,370 times.
		- At 30 each, she will need more than two weeks.
	- Nelle is not going to make her deadline.
	- Let's see if the Bash terminal can help her.

## Navigating Files and Directories
- The `$` sign is the prompt. It tells us the shell if waiting for input.

- We can find out who we are.

```bash
$ whoami
nelle
```

```bash
$ mycommand
mycommand: command not found
```

- The shell runs other commands or programs. If the program doesn't exist, it gives us an error.

- We can find out where we are.

```bash
$ pwd
/home/nelle/Desktop
```

- We see the name of the directory where we are at and all the directories that it is contained within.

- Each directory is separated by a `/`.

- If you are using a Mac or Linux based OS, the first `/` is considered the "root" of the file system.

- We call this an *absolute path* because our location is described relative to the whole file system.

- I can use this path to move to this location from any location on the computer.

- To change locations, use `cd` (change directory).

```bash
$ cd
$ pwd
/home/nelle
```

- `cd` by itself always takes us home.

- Give `cd` a path to change locations to a specific directory.

```bash
$ cd Downloads/data-shell/
$ pwd
/home/nelle/Desktop/data-shell
```

- Here we gave `cd` a *relative path* since the path was based on our current location.
	- Notice the path started with `Downloads` instead of `/` (or the C drive).
	- When using relative paths, our commands can only "see" the directories and files within the directory we are currently in.
	- Absolute paths are like street addresses, you can find the location from anywhere else.
	- Relative paths are like getting local street directions: "from the park, turn south, then take the third left."

- We can "list" all the files within the current directory.

```bash
$ ls
creatures  data  molecules  north-pacific-gyre  notes.txt  pizza.cfg  solar.pdf  writing
$ ls -F
creatures/  molecules/           notes.txt  solar.pdf
data/       north-pacific-gyre/  pizza.cfg  writing/
```

- `-F` is flag/option. It acts like a switch that changes some aspect of a commands behavior.

- `ls -F` puts a `/` mark behind all directories so we can distinguish them from files.

- We can use the `--help` flag to find out about `ls`'s flags.

```bash
$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~

    .
    .
    .
```

- What happens if we use an option that doesn't exist?

```bash
$ ls -j
ls: invalid option -- 'j'
Try 'ls --help' for more information.
```

> Challenge: What do the `-R` and `-l` options do with `ls`?

```bash
$ ls -R
.:
creatures  data  molecules  north-pacific-gyre  notes.txt  pizza.cfg  solar.pdf  writing

./creatures:
basilisk.dat  unicorn.dat

./data:
amino-acids.txt  animals.txt  morse.txt  planets.txt  sunspot.txt
animal-counts    elements     pdb        salmon.txt

./data/animal-counts:
animals.txt

./data/elements:
Ac.xml  Bi.xml  Co.xml  Fm.xml  H.xml   Md.xml  Np.xml  Pt.xml  Sc.xml  Te.xml  Y.xml
Ag.xml  Bk.xml  Cr.xml  Fr.xml  In.xml  Mg.xml  N.xml   Pu.xml  Se.xml  Th.xml  Zn.xml
Al.xml  Br.xml  Cs.xml  F.xml   Ir.xml  Mn.xml  Os.xml  P.xml   Si.xml  Ti.xml  Zr.xml
Am.xml  B.xml   Cu.xml  Ga.xml  I.xml   Mo.xml  O.xml   Ra.xml  Sm.xml  Tl.xml
Ar.xml  Ca.xml  C.xml   Gd.xml  Kr.xml  Na.xml  Pa.xml  Rb.xml  Sn.xml  Tm.xml
As.xml  Cd.xml  Dy.xml  Ge.xml  K.xml   Nb.xml  Pb.xml  Re.xml  Sr.xml  U.xml
At.xml  Ce.xml  Er.xml  He.xml  La.xml  Nd.xml  Pd.xml  Rh.xml  S.xml   V.xml
Au.xml  Cf.xml  Es.xml  Hf.xml  Li.xml  Ne.xml  Pm.xml  Rn.xml  Ta.xml  W.xml
Ba.xml  Cl.xml  Eu.xml  Hg.xml  Lr.xml  Ni.xml  Po.xml  Ru.xml  Tb.xml  Xe.xml
Be.xml  Cm.xml  Fe.xml  Ho.xml  Lu.xml  No.xml  Pr.xml  Sb.xml  Tc.xml  Yb.xml

./data/pdb:
aldrin.pdb          cyclopropane.pdb      methane.pdb        quinine.pdb
ammonia.pdb         ethane.pdb            methanol.pdb       strychnine.pdb
ascorbic-acid.pdb   ethanol.pdb           mint.pdb           styrene.pdb
benzaldehyde.pdb    ethylcyclohexane.pdb  morphine.pdb       sucrose.pdb
camphene.pdb        glycol.pdb            mustard.pdb        testosterone.pdb
cholesterol.pdb     heme.pdb              nerol.pdb          thiamine.pdb
cinnamaldehyde.pdb  lactic-acid.pdb       norethindrone.pdb  tnt.pdb
citronellal.pdb     lactose.pdb           octane.pdb         tuberin.pdb
codeine.pdb         lanoxin.pdb           pentane.pdb        tyrian-purple.pdb
cubane.pdb          lsd.pdb               piperine.pdb       vanillin.pdb
cyclobutane.pdb     maltose.pdb           propane.pdb        vinyl-chloride.pdb
cyclohexanol.pdb    menthol.pdb           pyridoxal.pdb      vitamin-a.pdb

./molecules:
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb

./north-pacific-gyre:
2012-07-03

./north-pacific-gyre/2012-07-03:
goodiff         NENE01736A.txt  NENE01843A.txt  NENE01978B.txt  NENE02040Z.txt
goostats        NENE01751A.txt  NENE01843B.txt  NENE02018B.txt  NENE02043A.txt
NENE01729A.txt  NENE01751B.txt  NENE01971Z.txt  NENE02040A.txt  NENE02043B.txt
NENE01729B.txt  NENE01812A.txt  NENE01978A.txt  NENE02040B.txt

./writing:
data  haiku.txt  thesis  tools

./writing/data:
LittleWomen.txt  one.txt  two.txt

./writing/thesis:
empty-draft.md

./writing/tools:
format  old  stats

./writing/tools/old:
oldtool
```

```bash
$ ls -l
total 76
drwxrwxr-x 2 nelle nelle  4096 Sep 15 10:00 creatures
drwxrwxr-x 5 nelle nelle  4096 Sep 15 10:00 data
drwxrwxr-x 2 nelle nelle  4096 Sep 15 10:00 molecules
drwxrwxr-x 3 nelle nelle  4096 Sep 15 10:00 north-pacific-gyre
-rw-rw-r-- 1 nelle nelle    86 Sep 15 10:00 notes.txt
-rw-rw-r-- 1 nelle nelle    32 Sep 15 10:00 pizza.cfg
-rw-rw-r-- 1 nelle nelle 21583 Sep 15 10:00 solar.pdf
drwxrwxr-x 5 nelle nelle  4096 Sep 15 10:00 writing
```

- If you want to find out more about a command's options, you can use `man` or Google.

- We can use `ls` to peek inside of a directory.

```bash
$ ls molecules/
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

```bash
$ cd data/
$ pwd
/home/nelle/Desktop/data-shell/data
```

- We know how to move down a level, but how do we go back up to the containing directory?

```bash
/home/phillip/Desktop/data-shell/data
$ cd data-shell
bash: cd: data-shell: No such file or directory
```

- We need to use a shell shortcut.

```bash
$ cd ..
$ pwd
/home/nelle/Desktop/data-shell
```

- `..` is shorthand for "the directory above where I am now."

```bash
$ ls -a -F
./   .bash_profile  data/       north-pacific-gyre/  pizza.cfg  writing/
../  creatures/     molecules/  notes.txt            solar.pdf
```

- If we use the `-a` flag to display hidden files, we see that there is also `.`. This is shorthand for the current directory.

- We can also put all the flags together when we type a command.

```bash
$ ls -aF
./   .bash_profile  data/       north-pacific-gyre/  pizza.cfg  writing/
../  creatures/     molecules/  notes.txt            solar.pdf
```

- Other useful shortcuts when using the terminal:
	- `Tab` autocomplete
	- `~` stands for your home directory
	- The up arrow cycles through your history of commands
	- `Ctrl`-`C` stops execution and returns your prompt
	- `cd -` goes back to the previous directory you were in

- Let's take a look at Nelle's data

```bash
$ ls north-pacific-gyre/2012-07-03/
goodiff         NENE01736A.txt  NENE01843A.txt  NENE01978B.txt  NENE02040Z.txt
goostats        NENE01751A.txt  NENE01843B.txt  NENE02018B.txt  NENE02043A.txt
NENE01729A.txt  NENE01751B.txt  NENE01971Z.txt  NENE02040A.txt  NENE02043B.txt
NENE01729B.txt  NENE01812A.txt  NENE01978A.txt  NENE02040B.txt
```

- Note the date convention for the directory with the data files.

## Working with Files and Directories
```bash
$ pwd
/home/nelle/Desktop/data-shell
```

- We're now ready to begin creating and destroying directories.

```bash
$ ls
creatures  data  molecules  north-pacific-gyre  notes.txt  pizza.cfg  solar.pdf  writing
$ mkdir thesis
$ ls
creatures  molecules           notes.txt  solar.pdf  writing
data       north-pacific-gyre  pizza.cfg  thesis
```

- Don't use spaces in you file names.

- You can use `_` or `-` instead.

- There is nothing in `thesis` yet.

```bash
$ ls thesis/
```

- `nano` is a text-based text editor that we can use to create a file.

```bash
$ nano draft.txt
  GNU nano 2.5.3                  File: dreaft.txt                                Modified  

It's not "publish or perish" anymore,
it's "share and thrive."




^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line
```

- Type `Ctrl`-`X` to exit.
- Type `Y` to save changes.
- Hit `Enter` to keep the filename the same.

```bash
$ ls
draft.txt
```

- To see what's inside the file use `cat`.

```bash
$ cat draft.txt 
It's not "publish or perish" anymore,
it's "share and thrive."
```

- To delete a file use `rm` (remove).

```bash
$ rm draft.txt 
$ ls
```

- Remove is not like the recycle bin, it's permanant, so be careful!

- Let's move up a directory and recreate the file.

```bash
$ nano draft.txt
  GNU nano 2.5.3                  File: draft.txt                                 Modified  

"The problem with quotes on the Internet is that it is hard to verify their authenticity."

~ Abraham Lincoln


^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line
```

- since `draft.txt` is not longer in `thesis`, we can remove the directory.

```bash
$ rm thesis/
rm: cannot remove 'thesis/': Is a directory
```

- `rm` is designed to remove individual files.

- To remove directories, we need to use the recursive option.

```bash
$ rm -r thesis/
$ ls
creatures  draft.txt  north-pacific-gyre  pizza.cfg  writing
data       molecules  notes.txt           solar.pdf
```

- This removes directories and everything in them.

- For extra safety I can, and *should*, use the interactive option.

```bash
$ mkdir thesis
$ rm -ri thesis/
rm: remove directory 'thesis/'? n
$ ls
creatures  draft.txt  north-pacific-gyre  pizza.cfg  thesis
data       molecules  notes.txt           solar.pdf  writing
```

- Let's move `draft.txt` back into the `thesis` directory.

```bash
$ mv draft.txt thesis/
$ ls thesis/
draft.txt
```

- `draft.txt` is not an informative name. `mv` also allows us to rename files.

```bash
$ mv thesis/draft.txt thesis/quotes.txt
$ ls thesis/
quotes.txt
```

- `mv` will silenty overwrite files, but you can also use the `-i` option with `mv` as well.

- Let's move `quotes.txt` back to the current working directory.

```bash
$ mv thesis/quotes.txt .
$ ls
creatures  molecules           notes.txt  quotes.txt  thesis
data       north-pacific-gyre  pizza.cfg  solar.pdf   writing
$ ls thesis/
```

- Copy works similarly to `mv`, except it makes a copy of the file.

```bash
$ ls
creatures  molecules           notes.txt  quotes.txt  thesis
data       north-pacific-gyre  pizza.cfg  solar.pdf   writing
$ ls thesis/
quotations.txt
```

> Challenge: Use what you have learned to create a file-tree of all the files and directories in `data-shell`. Feel free to summarize portions of your tree with `...`.

```bash
$ tree .
.
├── creatures
│   ├── basilisk.dat
│   └── unicorn.dat
├── data
│   ├── amino-acids.txt
│   ├── animal-counts
│   │   └── animals.txt
│   ├── animals.txt
│   ├── elements
│   │   ├── Ac.xml
│   │   ├── Ag.xml
│   │   ├── ...
│   ├── morse.txt
│   ├── pdb
│   │   ├── aldrin.pdb
│   │   ├── ammonia.pdb
│   │   ├── ascorbic-acid.pdb
│   │   ├── ...
│   ├── planets.txt
│   ├── salmon.txt
│   └── sunspot.txt
├── molecules
│   ├── cubane.pdb
│   ├── ethane.pdb
│   ├── methane.pdb
│   ├── octane.pdb
│   ├── pentane.pdb
│   └── propane.pdb
├── north-pacific-gyre
│   └── 2012-07-03
│       ├── goodiff
│       ├── goostats
│       ├── NENE01729B.txt
│       ├── NENE01736A.txt
│       ├── ...
├── notes.txt
├── pizza.cfg
├── quotes.txt
├── solar.pdf
├── thesis
│   └── quotations.txt
└── writing
    ├── data
    │   ├── LittleWomen.txt
    │   ├── one.txt
    │   └── two.txt
    ├── haiku.txt
    ├── thesis
    │   └── empty-draft.md
    └── tools
        ├── format
        ├── old
        │   └── oldtool
        └── stats

14 directories, 198 files
```

## Pipes and Filters
- We now know a few basic commands.

- We will now see how to put basic commands together into powerful workflows.

- Unix systems are organized around a "small pieces, loosly joined" philosophy.
	- Each tool does one thing (well) and one thing only.
	- Multiple tools are put together to do complex tasks.

- We will begin in the `molecules` directory.

```bash
$ cd molecules/
$ ls
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

- We have several "protein database files." Let's do a word count on each of them.

```bash
$ wc *.pdb
  20  156 1158 cubane.pdb
  12   84  622 ethane.pdb
   9   57  422 methane.pdb
  30  246 1828 octane.pdb
  21  165 1226 pentane.pdb
  15  111  825 propane.pdb
 107  819 6081 total
```

- `wc` gives us the number lines, words, and characters in each file.

- `*` is a *wildcard character*. It matches any number of characters.

- `wc -l` gives us just the number of lines for each file.

```bash
$ wc -l *.pdb
  20 cubane.pdb
  12 ethane.pdb
   9 methane.pdb
  30 octane.pdb
  21 pentane.pdb
  15 propane.pdb
 107 total
```

- If there were thousands of files, how would we know which one was the shortest?

- Begin by saving the word counts into a file using *redirect* (`>`).

```bash
$ wc -l *.pdb > lengths.txt
$ ls
cubane.pdb  ethane.pdb  lengths.txt  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

> How do we check that it worked?

```bash
$ cat lengths.txt 
  20 cubane.pdb
  12 ethane.pdb
   9 methane.pdb
  30 octane.pdb
  21 pentane.pdb
  15 propane.pdb
 107 total
```

- We can now sort the contents of the file using `sort`.

```bash
$ sort -n lengths.txt 
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
```

- The first one on the list is now the shortest.

- `head` can be used to get the first line from a file.

```bash
$ sort -n lengths.txt > sorted-length.txt
$ head -n 1 sorted-length.txt 
   9 methane.pdb
```

- Saving all these intermediate files along the way is confusing.

- The pipe (`|`) character take output from one command and "pipes" it into the input of the next command.

```bash
$ sort -n lengths.txt | head -n 1
   9 methane.pdb
```

- Let's recreate the whole work flow using pipes.

```bash
$ wc -l *.pdb
  20 cubane.pdb
  12 ethane.pdb
   9 methane.pdb
  30 octane.pdb
  21 pentane.pdb
  15 propane.pdb
 107 total
$ wc -l *.pdb | sort -n
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
$ wc -l *.pdb | sort -n | head -n 1
   9 methane.pdb
```

- Notice how we can build the workflow one piece at a time, checking that each step works.

> Challenge: All of Nelle's data files should have 300 lines. Change directory into `data-shell/north-pacific-gyre/2012-07-03` and create a pipeline that checks if there are any files that are too short.

```bash
$ cd ../north-pacific-gyre/2012-07-03/
$ wc -l *.txt
  300 NENE01729A.txt
  300 NENE01729B.txt
  300 NENE01736A.txt
  300 NENE01751A.txt
  300 NENE01751B.txt
  300 NENE01812A.txt
  300 NENE01843A.txt
  300 NENE01843B.txt
  300 NENE01971Z.txt
  300 NENE01978A.txt
  300 NENE01978B.txt
  240 NENE02018B.txt
  300 NENE02040A.txt
  300 NENE02040B.txt
  300 NENE02040Z.txt
  300 NENE02043A.txt
  300 NENE02043B.txt
 5040 total
$ wc -l *.txt | sort -n
  240 NENE02018B.txt
  300 NENE01729A.txt
  300 NENE01729B.txt
  300 NENE01736A.txt
  300 NENE01751A.txt
  300 NENE01751B.txt
  300 NENE01812A.txt
  300 NENE01843A.txt
  300 NENE01843B.txt
  300 NENE01971Z.txt
  300 NENE01978A.txt
  300 NENE01978B.txt
  300 NENE02040A.txt
  300 NENE02040B.txt
  300 NENE02040Z.txt
  300 NENE02043A.txt
  300 NENE02043B.txt
 5040 total
$ wc -l *.txt | sort -n | head -n 5
  240 NENE02018B.txt
  300 NENE01729A.txt
  300 NENE01729B.txt
  300 NENE01736A.txt
  300 NENE01751A.txt
```

- One of the files is 60 lines shorter than the others. Nelle sees that she did that assay at 8:00 am on a Monday morning. Someone was probably in using the machine on the weekend and she forgot to reset it.

- We can also check if there are any files that are too long using `tail`.

```bash
$ wc -l *.txt | sort -n | tail -n 5
  300 NENE02040B.txt
  300 NENE02040Z.txt
  300 NENE02043A.txt
  300 NENE02043B.txt
 5040 total
```

- All is well, but one of our files end in a "Z" instead of "A" or "B".

- Nelle informs us that her lab uses "Z" to indicate samples with missing information.

> Challenge: Help Nelle find all the files that end with "Z".

```bash
$ ls *Z.txt
NENE01971Z.txt  NENE02040Z.txt
```

- Nelle checks the log and realizes those two samples are missing depth recordings.

- She informs you there are other analyses she can use them for so she can't delete them.

- Another wildcard expression will allow us to select just files that end in "A" or "B"

```bash
$ ls *[AB].txt
NENE01729A.txt  NENE01751A.txt  NENE01843A.txt  NENE01978B.txt  NENE02040B.txt
NENE01729B.txt  NENE01751B.txt  NENE01843B.txt  NENE02018B.txt  NENE02043A.txt
NENE01736A.txt  NENE01812A.txt  NENE01978A.txt  NENE02040A.txt  NENE02043B.txt
```

## Loops
- Loops allow us to execute the same command repeatedly.

- Loops reduce typing and allow for increased automation.

```bash
$ pwd
/home/nelle/Desktop/data-shell/creatures
```

- Let's create a loop that outputs the first 3 lines of each file in the directory.

```bash
$ for filename in basilisk.dat unicorn.dat 
> do
>   head -n 3 $filename
> done
COMMON NAME: basilisk
CLASSIFICATION: basiliscus vulgaris
UPDATED: 1745-05-02
COMMON NAME: unicorn
CLASSIFICATION: equus monoceros
UPDATED: 1738-11-24
```

- Explain the different parts of a loop and what they do.

- Talk about good variable naming conventions (filename versus x, or temperature)

- Let's make our loop slighly more complicated.

```bash
$ for filename in *.dat
> do
>   echo $filename
>   head -n 100 $filename | tail -n 20
> done
basilisk.dat
CGGTACCGAA
AAGGGTCGCG
CAAGTGTTCC
CGGGACAATA
GTTCTGCTAA
GATAAGTATG
TGCCGACTTA
CCCGACCGTC
TAGGTTATAA
GGCACAACCG
CTTCACTGTA
GAGGTGTACA
AGGATCCGTT
GCGCGGGCGG
CAGTCTATGT
TTTTCGACAC
TGGACTGCTT
CCCTTTGAGG
GTGGATTTTT
CGTAACGGGT
unicorn.dat
CGGTACCGAA
AAGGGTCGCG
CAAGTGTTCC
CGGGACAATA
GTTCTGCTAA
GATAAGTATG
TGCCGACTTA
CCCGACCGTC
TAGGTTATAA
GGCACAACCG
CTTCACTGTA
GAGGTGTACA
AGGATCCGTT
GCGCGGGCGG
CAGTCTATGT
TTTTCGACAC
TGGACTGCTT
CCCTTTGAGG
GTGGATTTTT
CGTAACGGGT
```
- Discuss shell expansion and when it happens.

- `echo` prints whatever it is given.

- Whitespace is used to separate items in a list.

> Challenge: What happens when we try to use wildcards to copy multiple files?
> `$ cp *.dat original-*.dat`
> Why? How can we fix this with a loop?

```bash
$ cp *.dat original-*.dat
cp: target 'original-*.dat' is not a directory
```

- Since wildcards are expanded by the shell before the command is executed, we have confused `cp`.

```bash
$ for filename in *.dat
> do
>   cp $filename original-$filename
> done
$ ls
basilisk.dat  original-basilisk.dat  original-unicorn.dat  unicorn.dat
```

- Nelle is ready to begin analyzing her files using `goostats`.

- `goostats` calculates some statistics from a protein file.

- The program is run by typing `bash goostats` and giving it two arguements: the file you are analyzing, and the output file where the data is saved.

- Let's build the loop in steps using `echo`.

```bash
$ cd ../north-pacific-gyre/2012-07-03/
$ for datafile in NENE*[AB].txt
> do
>   echo $datafile
> done
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
NENE01751A.txt
NENE01751B.txt
NENE01812A.txt
NENE01843A.txt
NENE01843B.txt
NENE01978A.txt
NENE01978B.txt
NENE02018B.txt
NENE02040A.txt
NENE02040B.txt
NENE02043A.txt
NENE02043B.txt
$ for datafile in NENE*[AB].txt; do echo $datafile stats-$datafile; done
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
NENE01751A.txt stats-NENE01751A.txt
NENE01751B.txt stats-NENE01751B.txt
NENE01812A.txt stats-NENE01812A.txt
NENE01843A.txt stats-NENE01843A.txt
NENE01843B.txt stats-NENE01843B.txt
NENE01978A.txt stats-NENE01978A.txt
NENE01978B.txt stats-NENE01978B.txt
NENE02018B.txt stats-NENE02018B.txt
NENE02040A.txt stats-NENE02040A.txt
NENE02040B.txt stats-NENE02040B.txt
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
$ for datafile in NENE*[AB].txt; do echo bash goostats $datafile stats-$datafile; done
bash goostats NENE01729A.txt stats-NENE01729A.txt
bash goostats NENE01729B.txt stats-NENE01729B.txt
bash goostats NENE01736A.txt stats-NENE01736A.txt
bash goostats NENE01751A.txt stats-NENE01751A.txt
bash goostats NENE01751B.txt stats-NENE01751B.txt
bash goostats NENE01812A.txt stats-NENE01812A.txt
bash goostats NENE01843A.txt stats-NENE01843A.txt
bash goostats NENE01843B.txt stats-NENE01843B.txt
bash goostats NENE01978A.txt stats-NENE01978A.txt
bash goostats NENE01978B.txt stats-NENE01978B.txt
bash goostats NENE02018B.txt stats-NENE02018B.txt
bash goostats NENE02040A.txt stats-NENE02040A.txt
bash goostats NENE02040B.txt stats-NENE02040B.txt
bash goostats NENE02043A.txt stats-NENE02043A.txt
bash goostats NENE02043B.txt stats-NENE02043B.txt
```

- It looks right, let's run it!

```bash
$ for datafile in NENE*[AB].txt; do bash goostats $datafile stats-$datafile; done
^C
```

- It's running ... I think. Use `Ctrl`-`C` to stop the loop.

- Let's try again, but add an `echo` statement so that we get a status update.

```bash
$ for datafile in NENE*[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
NENE01751A.txt
NENE01751B.txt
NENE01812A.txt
NENE01843A.txt
NENE01843B.txt
NENE01978A.txt
NENE01978B.txt
NENE02018B.txt
NENE02040A.txt
NENE02040B.txt
NENE02043A.txt
NENE02043B.txt
```

## Shell Scripts
- In order to make our workflow reproducable we save a workflow into a text file and recall the entire workflow any time we need it.

- This makes it much faster to repeat a task next time we need to do it.

- Let's create a script that returns the middle lines of a file.

```bash
$ cd ../../molecules/
$ ls
cubane.pdb  lengths.txt  octane.pdb   propane.pdb
ethane.pdb  methane.pdb  pentane.pdb  sorted-length.txt
$ nano middle.sh

  GNU nano 2.5.3                  File: middle.sh                                           

head -n 15 octane.pdb | tail -n 5





                                      [ Read 1 line ]
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Linter   ^_ Go To Line

$ ls
cubane.pdb  lengths.txt  middle.sh   pentane.pdb  sorted-length.txt
ethane.pdb  methane.pdb  octane.pdb  propane.pdb
```

- we now ask the shell to execute the script.

```bash
$ bash middle.sh 
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

- It executed the commands in the file.

- This script will always give us the middle of the `octane.pdb` file. Let's make it more versatile.

```bash
$ nano middle.sh

  GNU nano 2.5.3                  File: middle.sh                                 Modified  

head -n 15 "$1" | tail -n 5

                                      [ Read 1 line ]
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Linter   ^_ Go To Line
```

- `$1` is a special variable. It tells bash to take the first argument and put it in that place in the script.

- We surround `$1` in quotes in case somebody (else) puts a space in their argument.

```bash
$ bash middle.sh cubane.pdb 
ATOM      9  H           1       1.410  -1.631   0.942  1.00  0.00
ATOM     10  H           1      -0.262  -2.112  -1.024  1.00  0.00
ATOM     11  H           1      -2.224  -0.925   0.328  1.00  0.00
ATOM     12  H           1      -0.468  -0.501   2.315  1.00  0.00
ATOM     13  H           1       2.224   0.892  -0.134  1.00  0.00
$ bash middle.sh propane.pdb 
ATOM      9  H           1      -0.914   0.551  -1.359  1.00  0.00
ATOM     10  H           1      -1.396   1.211   0.219  1.00  0.00
ATOM     11  H           1      -2.058  -0.345  -0.332  1.00  0.00
TER      12              1
END
```

- Let's make our script even more useful by allowing for a change in the range of lines we select from the file.

```bash
$ nano middle.sh

  GNU nano 2.5.3                  File: middle.sh                                           

head -n "$2" "$1" | tail -n "$3"


                                      [ Read 1 line ]
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Linter   ^_ Go To Line

$ bash middle.sh pentane.pdb 15 5
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
$ bash middle.sh pentane.pdb 16 2
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
```

- `#` indicates a comment in bash.

- The computer ignores lines that start with `#` (for human eyes only).

- It is important to use comments in your scripts so others can use them (and you can six months later).

```bash
$ nano middle.sh

  GNU nano 2.5.3                  File: middle.sh                                 Modified  

# Name: middle.sh
# Usage: bash sorted.sh filename end_line num_lines
# Description: Select lines from the middle of a file.

head -n "$2" "$1" | tail -n "$3"



^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Linter   ^_ Go To Line
```

- What if we want to process many files with one script?

> Challenge: Use pipes to create a script that sorts files by length in both this directory and the `data-shell/creatures` directory.

```bash
$ wc -l *.pdb ../creatures/*.dat
   20 cubane.pdb
   12 ethane.pdb
    9 methane.pdb
   30 octane.pdb
   21 pentane.pdb
   15 propane.pdb
  163 ../creatures/basilisk.dat
  163 ../creatures/original-basilisk.dat
  163 ../creatures/original-unicorn.dat
  163 ../creatures/unicorn.dat
  759 total
$ wc -l *.pdb ../creatures/*.dat | sort -n
    9 methane.pdb
   12 ethane.pdb
   15 propane.pdb
   20 cubane.pdb
   21 pentane.pdb
   30 octane.pdb
  163 ../creatures/basilisk.dat
  163 ../creatures/original-basilisk.dat
  163 ../creatures/original-unicorn.dat
  163 ../creatures/unicorn.dat
  759 total
```

- Since the bash shell is interactive, we can play with things until they work and then save our history to a file to make setting up a script faster.

- Let's create a script that sorts any number of files by length.

```bash
$ history | tail -n 10 > sorted.sh
```

- We can use the special variable `$@` which will take any number of arguments (we don't have to know how many).

```bash
$ nano sorted.sh

  GNU nano 2.5.3                  File: sorted.sh                                 Modified  

# Name: sorted.sh
# Usage: bash sorted.sh one_or_more_filenames
# Description: Sort filenames by their length.

wc -l "$@" | sort -n





^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Linter   ^_ Go To Line

$ bash sorted.sh *.pdb ../creatures/*.dat ../data/elements/*.xml
    7 ../data/elements/Bk.xml
    7 ../data/elements/Cf.xml
    7 ../data/elements/Cm.xml
    7 ../data/elements/Es.xml
    7 ../data/elements/Fm.xml
    7 ../data/elements/Lr.xml
    7 ../data/elements/Md.xml
    7 ../data/elements/No.xml
    8 ../data/elements/Ac.xml
    8 ../data/elements/At.xml
    8 ../data/elements/La.xml
    9 ../data/elements/Ag.xml
    9 ../data/elements/Al.xml
    9 ../data/elements/Am.xml
    9 ../data/elements/Ar.xml
    9 ../data/elements/As.xml
    9 ../data/elements/Au.xml
    9 ../data/elements/Ba.xml
    .
    .
    .
    .
    9 ../data/elements/U.xml
    9 ../data/elements/V.xml
    9 ../data/elements/W.xml
    9 ../data/elements/Xe.xml
    9 ../data/elements/Yb.xml
    9 ../data/elements/Y.xml
    9 ../data/elements/Zn.xml
    9 ../data/elements/Zr.xml
    9 methane.pdb
   12 ethane.pdb
   15 propane.pdb
   20 cubane.pdb
   21 pentane.pdb
   30 octane.pdb
  163 ../creatures/basilisk.dat
  163 ../creatures/original-basilisk.dat
  163 ../creatures/original-unicorn.dat
  163 ../creatures/unicorn.dat
 1667 total
```

> Homework (optional): Create a script that makes Nelle's analysis from the previous section reproducible.

```bash
# Calculate stats for Site A and Site B data files.
for datafile in NENE*[AB].txt
do
    echo $datafile
    bash goostats $datafile stats-$datafile
done
```

## Finding Things
- One final skill that is useful on the command line is finding things.

- There are two tools for finding things: `grep` and `find`.

```bash
$ cd ../writing/
$ cat haiku.txt 
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
```

- `grep` searches content within files.

- We use it by typing `grep`, followed by the search term, and then the file that we want to look in.

```bash
$ grep not haiku.txt 
Is not the true Tao, until
"My Thesis" not found.
Today it is not working
```

```bash
$ grep The haiku.txt 
The Tao that is seen
"My Thesis" not found.
```

- `grep` does not pay attention to word boundaries by default. We can change this by using the `-w` option.

```bash
$ grep -w The haiku.txt 
The Tao that is seen
```

- `grep` defines a word as anything separated by a space. If we want to find a phrase, we need to put it in quotes.

```bash
$ grep -w "is not" haiku.txt 
Today it is not working
```

- `grep` can tell us the line number where the search term was found.

```bash
$ grep -n "it" haiku.txt 
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
```

- `grep` is case-sensitive by default. We can make it case *insensitive*.

```bash
$ grep -nwi "the" haiku.txt 
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
```

- We can *invert* our search and find all the lines that **don't** have "the".

```bash
$ grep -nwv "the" haiku.txt 
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
```

- These options are only the beginning ...

```bash
$ grep --help
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
  -s, --no-messages         suppress error messages
  -v, --invert-match        select non-matching lines
  -V, --version             display version information and exit
      --help                display this help text and exit
   .
   .
   .
```

- `find` is used to find files themselves.

- To use `find`, type `find`, followed by where you want to look, then the search terms/options.

```bash
$ find .
.
./data
./data/two.txt
./data/LittleWomen.txt
./data/one.txt
./thesis
./thesis/empty-draft.md
./haiku.txt
./tools
./tools/stats
./tools/old
./tools/old/oldtool
./tools/format
```

- We can look for only directories.

```bash
$ find . -type d
.
./data
./thesis
./tools
./tools/old
```

- We can search for only files.

```bash
$ find . -type f
./data/two.txt
./data/LittleWomen.txt
./data/one.txt
./thesis/empty-draft.md
./haiku.txt
./tools/stats
./tools/old/oldtool
./tools/format
```

- We can search for a specific file. Let's find all the text files.

```bash
$ find . -name *.txt
./haiku.txt
```

- What happened?

- Remember that the shell expands wildcards first before executing commands. Here's how to fix it:

```bash
$ find . -name '*.txt'
./data/two.txt
./data/LittleWomen.txt
./data/one.txt
./haiku.txt
```

- We can use the `$()` construction to use `grep` and `find` together.

```bash
$ grep "FE" $(find .. -name '*.pdb')
../data/pdb/heme.pdb:ATOM     25 FE           1      -0.924   0.535  -0.518
```
