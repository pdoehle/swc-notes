# Version Control with Git

## Creating a Repository
- Create a directory and initialize a `git` project.

```bash
$ cd 
$ cd Desktop/
$ mkdir planets
$ cd planets/
$ git init
Initialized empty Git repository in /home/nelle/Desktop/planets/.git/
```

- Creates a hidden file, `.git`, that keeps a record of all our project changes.

```bash
$ ls -a
.  ..  .git
```

- If we delete this directory, we will lose the hitory of our project.

- Check that our project is up to date.

```bash
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

## Tracking Changes
- Let's create a file for our project.

```bash
$ nano mars.txt

  GNU nano 2.5.3                    File: mars.txt                                                

Cold and dry, but everything is my favorite color.


                                         [ Read 1 line ]
^G Get Help     ^O Write Out    ^W Where Is     ^K Cut Text     ^J Justify      ^C Cur Pos
^X Exit         ^R Read File    ^\ Replace      ^U Uncut Text   ^T To Spell     ^_ Go To Line

$ ls
mars.txt
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
```

- If we check the status, we see that there is a new file.

```bash
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	mars.txt

nothing added to commit but untracked files present (use "git add" to track)
```

- `git` is not tracking the file.

- We can tell `git` to start tracking the file.

```bash
$ git add mars.txt 
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   mars.txt

```

- Git is now tracking changes for `mars.txt`, but it hasn't recorded the changes for a commit.

```bash
$ git commit -m "Start notes on Mars as a base"
[master (root-commit) 08c7c61] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
```

- Git "takes a snapshot" of our project and saves a copy of it in `.git`.

- Good commit messages ...
	- start with a brief (<50 characters) comment,
	- a longer descripton can follow separated by a blank line.

- The initial comment should ...
	- be written in the imperative mood,
	- start with a capital letter,
	- *not* end with a period

- Git is now up to date with our current status of the project.

```bash
$ git status
On branch master
nothing to commit, working directory clean
```

- We can see the projects history using `git log`.

```bash
$ git log
commit 08c7c618152ab91b4e6589a6f2a2407e1cc38868
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 15:36:37 2018 -0600

    Start notes on Mars as a base
```

- Let's make a change to our file.

```bash
$ nano mars.txt 
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
The two moons may be a problem for Wolfman.
``` 

- Once again, `git`, notes the changes, but does not track them.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- We still need to tell `git` that we want to pay attention (`git add`) and save (`git commit`) the changes.

- It's good practice to review our changes before saving them.

```bash
$ git diff
diff --git a/mars.txt b/mars.txt
index 077071c..42d92e3 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color.
+The two moons may be a problem for Wolfman.
```

- Shows the difference between the state of the file and the most recently saved version.

- Line 1: output similar to the Unix `diff` command.

- Line 2: which versions of the file Git is comparing.

- Line 3-4: human friendly printout of the files being compared.

- Remaining: actual differences between the two files.

- Let's commit to these new changes.

```bash
$ git add mars.txt 
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
[master 3899153] Add concerns about effects of Mars' moons on Wolfman
 1 file changed, 1 insertion(+)
```

- Notice `git` has a two-step process of staging and then committing?

- We can think of this like a family portrait.
	- staging is getting everybody in the fram
	- committing is taking the photo

- Why the two-step process? Why not just commit?

- Staging allows us to commit to small pieces of our project at one time instead of large portions

- If you are working on a paper, you can commit to just the introduction if the conclusion is not ready yet

- Let's watch our changes move through the progression ...

- Make another change to `mars.txt`

```bash
$ nano mars.txt 
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
The two moons may be a problem for Wolfman.
But the Mummy will appreciate the lack of humidity
$ git diff
diff --git a/mars.txt b/mars.txt
index 42d92e3..669e484 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color.
 The two moons may be a problem for Wolfman.
+But the Mummy will appreciate the lack of humidity
$ git add mars.txt 
$ git diff
```

- There is no output.

- `git diff` compares the staged content with the state of your current directory.

- It is designed to help you compare what changes you have made that are *not* being tracked at all.

- Since all our most recent changes have been staged, there is no difference between what is in our directory and what's staged.

- There is an option for comparing staged commits with the last commit.

```bash
$ git diff --staged
diff --git a/mars.txt b/mars.txt
index 42d92e3..669e484 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color.
 The two moons may be a problem for Wolfman.
+But the Mummy will appreciate the lack of humidity
```

- Let's commit.

```bash
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
[master 3b3848c] Discuss concerns about Mars' climate for Mummy
 1 file changed, 1 insertion(+)
$ git status
On branch master
nothing to commit, working directory clean
```

- Our history log is now much more interesting.

```bash
$ git log
commit 3b3848ceb653b3f840f0d10d3a464ab2c307f042
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 17:09:09 2018 -0600

    Discuss concerns about Mars' climate for Mummy

commit 389915371d8ec7280a1d5fe8cb61862232893404
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 15:57:58 2018 -0600

    Add concerns about effects of Mars' moons on Wolfman

commit 08c7c618152ab91b4e6589a6f2a2407e1cc38868
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 15:36:37 2018 -0600

    Start notes on Mars as a base
```

- Most recent commits are listed first.

## Exploring History
- In order to take advantage of version control, we need to be able to navigate back to previous versions.

- Let's make an "ill-considered change."

```bash
$ git diff HEAD mars.txt
diff --git a/mars.txt b/mars.txt
index 669e484..0a33bf7 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,3 +1,4 @@
 Cold and dry, but everything is my favorite color.
 The two moons may be a problem for Wolfman.
 But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

- We are now comparing changes between our directory and the most recent commit (indicated by `HEAD`)

- `HEAD` is a nice shortcut as it allows us to avoid having to type the commit ID number.

- We can reference any number of changes back from head by following `HEAD` with a `~` and the number of changes back.

```bash
$ git diff HEAD~1 mars.txt
diff --git a/mars.txt b/mars.txt
index 42d92e3..0a33bf7 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,4 @@
 Cold and dry, but everything is my favorite color.
 The two moons may be a problem for Wolfman.
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
$ git diff HEAD~2 mars.txt
diff --git a/mars.txt b/mars.txt
index 077071c..0a33bf7 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color.
+The two moons may be a problem for Wolfman.
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

- We can refer to commits by their unique identifying number if we need to.

- Git will find the commit once we have enough unique digits to identify the commit.

```bash
$ git log
commit 3b3848ceb653b3f840f0d10d3a464ab2c307f042
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 17:09:09 2018 -0600

    Discuss concerns about Mars' climate for Mummy

commit 389915371d8ec7280a1d5fe8cb61862232893404
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 15:57:58 2018 -0600

    Add concerns about effects of Mars' moons on Wolfman

commit 08c7c618152ab91b4e6589a6f2a2407e1cc38868
Author: Phillip Doehle <doehle@okstate.edu>
Date:   Mon Jan 8 15:36:37 2018 -0600

    Start notes on Mars as a base
$ git diff 3899 mars.txt
diff --git a/mars.txt b/mars.txt
index 42d92e3..0a33bf7 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,4 @@
 Cold and dry, but everything is my favorite color.
 The two moons may be a problem for Wolfman.
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

- We may need to restore an old version of our project at some point.

- Let's "accidentally" overwrite our file.

```bash
$ nano mars.txt
$ cat mars.txt
We will need to manufacture our own oxygen.
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- We need to restore the last commit!

```bash
$ git checkout HEAD mars.txt
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
The two moons may be a problem for Wolfman.
But the Mummy will appreciate the lack of humidity
```

- We can go back even further by using a commit identifier or the `HEAD` notation.

```bash
$ git checkout HEAD~2 mars.txt
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   mars.txt


```

- The changes are staged so that we can commit to this old version going forward.

- Let's revert to what we had before this mess.

```bash
$ git checkout HEAD mars.txt
$ cat mars.txt 
Cold and dry, but everything is my favorite color.
The two moons may be a problem for Wolfman.
But the Mummy will appreciate the lack of humidity
$ git status
On branch master
nothing to commit, working directory clean
```

- Remember to use the commit identifier for the commit *before* the change we are tyring to undo.

## Ignoring Things
- Sometimes there are files that we want Git to ignore.

- Sometimes we don't want to share files with collaborators.
	- Files created by our editor
	- Intermediate files during data analysis
	- Binary files

- Let's make some files that Git will ignore.

```bash
$ mkdir results
$ touch a.dat b.dat c.dat results/a.out results/b.out
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.dat
	b.dat
	c.dat
	results/

nothing added to commit but untracked files present (use "git add" to track)
$ nano .gitignore
$ cat .gitignore 
*.dat
results/
```

- Now Git will not even attempt to track these files.

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

- We only see that a new `.gitignore` file has been created.

- Let's commit to it.

```bash
$ git add .gitignore 
$ git commit -m "Add the ignore file"
[master d563048] Add the ignore file
 1 file changed, 2 insertions(+)
 create mode 100644 .gitignore
$ git status
On branch master
nothing to commit, working directory clean
```

- `.gitignore` will help prevent us from accidentally committing files we already told it to ignore.

```bash
$ git add a.dat 
The following paths are ignored by one of your .gitignore files:
a.dat
Use -f if you really want to add them.
```

- Git forces us to use an `-f` flag if we really want to track the file.

- If we need a reminder of what files have been ignored, we can use the `--ignored` flag.

```bash
$ git status --ignored
On branch master
Ignored files:
  (use "git add -f <file>..." to include in what will be committed)

	a.dat
	b.dat
	c.dat
	results/

nothing to commit, working directory clean
```


