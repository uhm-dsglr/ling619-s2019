# Introduction to Git (Part 1)

The materials below have been adapted from the excellent lessons by the Software Carpentry, which they have generously made available through the [CC BY 4.0 license](https://creativecommons.org/licenses/by/4.0/). For each of the sections, we encourage you to visit [Software Carpentry's original lesson page](http://swcarpentry.github.io/git-novice/) for more in-depth content. Our workshop lessons below are based on their lessons 2 to 6. 

1.  [Setting Up Git](#setting-up-git)  
1.  [Creating a Repository](#creating-a-repository) 
1.  [Tracking Changes](#tracking-changes) 
1.  [A Commit Workflow](#a-commit-workflow) (2nd half of SWC's Tracking Changes)
1.  [Exploring History](#exploring-history) 
1.  [Ignoring Things](#ignoring-things) 

## Setting Up Git

When we use Git on a new computer for the first time,
we need to configure a few things. Below are a few examples
of configurations we will set as we get started with Git:

*   our name and email address,
*   what our preferred text editor is,
*   and that we want to use these settings globally (i.e. for every project).

On a command line, Git commands are written as `git verb options`,
where `verb` is what we actually want to do and `options` is additional optional information which may be needed for the `verb`. So here is how you would look up your global setting: 

```bash
$ git config --global --list
user.name=Bradley McDonnell
user.email=mcdonn@hawaii.edu
core.editor=nano
core.autocrlf=input
core.safecrlf=false
```

Your name and email will need setting up. Commands to set them on your machine:

```bash
$ git config --global user.name "Henry Higgins"
$ git config --global user.email "profhiggins@oxford.edu"
```
Please use your own name and email address instead of Prof. Higgins's. This user name and email will be associated with your subsequent Git activity,
which means that any changes pushed to online git host servers such as 
[GitHub](https://github.com/) 
in a later lesson will include this information.

One additional detail: your default editor. If you followed our installation instruction, it should already be set to `nano` or your own favorite text editor. If that is not the case, reset it as shown below. 

```bash
$ git config --global core.editor "nano"
```

Lastly, if you forget a `git` command, you can access the list of commands by using `-h` and access the Git manual by using `--help`:
```bash
$ git config -h
```


## Creating a Repository


Once Git is configured, we can start using it. First, let's create a directory in `Desktop` folder for our work and then move into that directory:

```bash
$ cd ~/Desktop
$ mkdir languages
$ cd languages
```


Then we tell Git to make `languages` a **repository** -- a place where
Git can store versions of our files:

```bash
$ git init
```


It is important to note that `git init` will create a repository that
includes subdirectories and their files -- there is no need to create
separate repositories nested within the `languages` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `languages` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```bash
$ ls
```

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `languages` called `.git`:

```bash
$ ls -a
.	..	.git
```

Git uses this special sub-directory to store all the information about the project, 
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` sub-directory,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

```bash
$ git status
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
```



## Tracking Changes

<!-- 
First let's make sure we're still in the right directory.
You should be in the `languages` directory.

```bash
$ pwd
/home/naraehan/Desktop/languages
```
-->


Let's create a file called `zulu.txt` that contains some notes
about the language.
I'll use `nano` to edit the file; you can use your favorite plain-text editor if you have one. 
(Note: It does not have to be the `core.editor` you set globally earlier.)  

```bash
$ nano zulu.txt
```

An editor window will open up. Type the text below into the `zulu.txt` file:

```
belongs to the Bantu language family
```

To save and exit `nano`,  hit `Ctrl+X`, and then `y` to save. `zulu.txt` now contains a single line, which we can view by running the `cat` ("concatenate") command:

```bash
$ cat zulu.txt
belongs to the Bantu language family
```

If we check the status of our project again, Git tells us that it’s noticed the new file:

```bash
$ git status
On branch master

Initial commit

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	zulu.txt
nothing added to commit but untracked files present (use "git add" to track)
```

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

```bash
$ git add zulu.txt
```
and then check that the right thing happened:

```bash
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   zulu.txt
```

Git now knows that it's supposed to keep track of `zulu.txt`, 
but it hasn't recorded these changes yet.
To get it to do that, we need to run `git commit`:

```bash
$ git commit -m "start notes on Zulu language"
[master (root-commit) f22b25e] start notes on Zulu language
 1 file changed, 1 insertion(+)
 create mode 100644 zulu.txt
```

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a **commit** and its short identifier is `f22b25e` in this example.

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

If we run `git status` now:

```bash
$ git status
On branch master
nothing to commit, working directory clean
```

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```bash
$ git log
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Henry Higgins <profhiggins@oxford.edu>
Date:   Thu Aug 22 09:51:46 2018 -0400

    start notes on Zulu language
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier (which starts with the same `f22b25e`),
the commit's author,
when it was created, and the log message Git was given when the commit was created.


Now suppose Prof. Higgins adds more information to the file:
<!--
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)
-->

```bash
$ nano zulu.txt
$ cat zulu.txt
belongs to the Bantu language family
spoken in South Africa
```

When we run `git status` now,
it tells us that a file it already knows about has been modified:

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   zulu.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
	
The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we actually saved them (which we do with `git commit`).
Before taking those steps, it is good practice to always review
our changes. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

```bash
$ git diff
diff --git a/zulu.txt b/zulu.txt
index df0654a..315bf3a 100644
--- a/zulu.txt
+++ b/zulu.txt
@@ -1 +1,2 @@
 belongs to the Bantu language family
+spoken in South Africa
```

The output is cryptic, but to break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added a line.

After reviewing our change, we go for committing:

```bash
$ git commit -m "add region information"
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   zulu.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that. We add *and then* commit:

```bash
$ git add zulu.txt
$ git commit -m "add region information"
[master 34961b1] add region information
 1 file changed, 1 insertion(+)
```

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example, suppose we're adding a few citations to relevant research to our thesis.
We might want to commit those additions,
and the corresponding bibliography entries,
but *not* commit some of our work drafting the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special **staging area**
where it keeps track of things that have been added to
the current **changeset** but not yet committed. 
If you think of Git as taking snapshots of changes over the life of a project,
`git add` specifies *what* will go in a snapshot
(putting things in the staging area),
and `git commit` then *actually takes* the snapshot, and
 makes a permanent record of it (as a commit). An illustration: 

<!-- 
> ### Staging Area
>
> If you think of Git as taking snapshots of changes over the life of a project,
> `git add` specifies *what* will go in a snapshot
> (putting things in the staging area),
> and `git commit` then *actually takes* the snapshot, and
> makes a permanent record of it (as a commit).
> If you don't have anything staged when you type `git commit`,
> Git will prompt you to use `git commit -a` or `git commit --all`,
> which is kind of like gathering *everyone* for the picture!
> However, it's almost always better to
> explicitly add things to the staging area, because you might
> commit changes you forgot you made. 
-->

<img src="http://swcarpentry.github.io/git-novice/fig/git-committing.svg">


