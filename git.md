# Version Control with Git

## Automated Version Control
- SLIDE

- SLIDE

- How many of you can relate to the poor graduate student in the comic?

- Git is a command-line based version control system.

- Version control systems you may already be familiar with: Microsoft Word's Track Changes, Google Docs' version history, or LibreOffice's Recording and Displaying Changes.

- SLIDE

- Version control systems start with a base document and then record each change separately, like the recording of a tape the can be rewound.

- SLIDE

- If you think of the changes as separate from the document itself, you can have multiple versions of the same document.

- If there is no conflict between different versions, you can even bring them back together as a unified document.

- Git is a version control system that manages this and more.

- It allows you to decide what changes are tracked and keeps track of each version (called *commits*) of your project.

- The complete history of commits for a particular project (along with their metadata) make up a *repository*.

## Setting Up Git

- SLIDE

- Go to [GitHub's](https://github.com/) webpage and setup an account if you don't already have one.

- Explain the difference between Git and GitHub

- Git commands are written as `git verb` (and optional flags)

- Suggested settings for this lesson:

```bash
$ git config --global user.name "Nelle Nemo"
$ git config --global user.email "nelle@university.edu"
$ git config --global color.ui "auto"
$ git config --global core.editor "nano -w"
```

- You can check that you have the correct settings:

```bash
$ git config --list
```

## Creating a Repository
- SLIDE

- Create a directory and initialize a `git` project.

```bash
$ cd 
$ cd Desktop/
$ mkdir planets
$ cd planets/
$ git init
Initialized empty Git repository in /home/nelle/Desktop/planets/.git/
```

- Creates a hidden file, `.git`, that keeps a record of all our project's changes.

```bash
$ ls -a
.  ..  .git
```

- If we delete this directory, we will lose the history of our project.

- Check that our project is up to date.

```bash
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

## Tracking Changes
- SLIDE

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

- SLIDE

- Good commit messages ...
	- start with a brief (<50 characters) comment,
	- a longer description can follow separated by a blank line.

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

- We can see the project's history using `git log`.

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

- Once again, `git` notes the changes, but does not track them.

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

- Shows the difference between our project as it stands and the files being tracked.

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

- SLIDE

- Notice `git` has a two-step process of staging and then committing.

- We can think of this like a wedding picture.
	- staging is getting everybody in the frame
	- committing is taking the photo

- Why the two-step process? Why not just commit?

- SLIDE

- Staging allows us to commit to small pieces of our project at one time instead of large portions

- If you are working on a paper, you can commit to just the introduction if the conclusion is not ready yet

- Let's see how this affects the Git workflow ...

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
- SLIDE

- In order to take advantage of version control, we need to be able to navigate back to previous versions.

- Let's make an "ill-considered change."

```bash
$ nano mars.txt
$ cat mars.txt
Cold and dry, but everything is my favorite color.
The two moons may be a problem for Wolfman.
But the Mummy will appreciate the lack of humidity
An ill-considered change.
```

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

- Remember to use the commit identifier for the commit *before* the change we are trying to undo.

## Ignoring Things
- SLIDE

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

## Remotes and Collaborating
- SLIDE

- Version control becomes particularly powerful when we use it in collaboration with others.

- Hosting services like [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/), or [GitLab](https://gitlab.com/) allow us to host our Git repos in a central repository and share them with collaborators.

- Git allows us to track who made what changes and when they made them, and even provides for resolving conflicts when working together on a file.

- Let's collaborate on a project together.

- Go to my GitHub page and click on the countries repo.

- Any repo that is public can be forked and you can work on your own version.

- Fork the repo into your own GitHub.

- Click the "Clone or download" button and copy the link to your clipboard.

- Past the link into your terminal and clone the repo to your local computer.

```bash
$ cd ~/Desktop/
$ git clone https://github.com/nelle/countries.git
Cloning into 'countries'...
remote: Counting objects: 207, done.
remote: Compressing objects: 100% (204/204), done.
remote: Total 207 (delta 3), reused 207 (delta 3), pack-reused 0
Receiving objects: 100% (207/207), 22.67 KiB | 0 bytes/s, done.
Resolving deltas: 100% (3/3), done.
Checking connectivity... done.
$ cd countries/
```

- Pick a country and type it on the Etherpad.

- If somebody already picked your country, you will need to pick a different one.

- Add an interesting fact in your country's file by using `nano` to edit the file.

```bash
$ cd countries/
$ nano China.md
```

- Commit your changes.

```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   China.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git add China.md 
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   China.md

$ git commit -m "Add official language"
[master 1c533c4] Add official language
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
```

- We are ready to push the changes up to the remote repository.

```bash
$ git push origin master 
Username for 'https://github.com': nelleuniversity.edu
Password for 'https://nelle@university.edu@github.com': 
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 431 bytes | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/OSU-Carpentry/countries.git
   f15fb28..1c533c4  master -> master
```

- The changes are now reflected in the remote repo.

- We can request to make changes to my original repo on GitHub by making a "pull request."

- Use the "Pull requests" tab and click "New pull request," to make a pull request.

- Make sure "base fork" is my countries repo, and "base" is "master."

- Make sure "head fork" is your countries repo and "compare" is "master."

- Click "Create pull request."

- Make your case and submit it.

- Owner can accept, ask for more detail, or decline the pull request.

- Once all the collaborative changes have been accepted, our local copies now need to reflect all the updates.

- Copy the url for my original repo.

```bash
$ git remote add upstream https://github.com/nelle/countries
$ git fetch upstream 
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (5/5), done.
From https://github.com/nelle/countries
 * [new branch]      master     -> upstream/master
$ git merge upstream/master 
Updating 1c533c4..baccecb
Fast-forward
 countries/United_Kingdom.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ cat United_Kingdom.md 
##United_Kingdom
## population


## capital

 
## official language
English

## interesting trivia
```

- Congratulations! We just collaborated on a GitHub project!
