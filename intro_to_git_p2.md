# Introduction to Git (Part 2)
## A Commit Workflow

<!-- 
Let's watch as our changes to a file move from our editor
to the staging area and into long-term storage. -->

Let's recap by trying another round, from start to finish. 
First, we'll add another line to the Sundanese file, this time about word order:

```bash
$ nano sundanese.txt
$ cat sundanese.txt
belongs to the Austronesian language family
spoken in western Indonesia
word order: SVO
```

Then see what's changed: 

```bash
$ git diff
diff --git a/sundanese.txt b/sundanese.txt
index 315bf3a..b36abfd 100644
--- a/sundanese.txt
+++ b/sundanese.txt
@@ -1,2 +1,3 @@
 belongs to the Austronesian language family
 spoken in western Indonesia
+word order: SVO
```

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column). (For a detailed description of the `git diff` report see [this explanation](https://www.git-tower.com/learn/git/ebook/en/command-line/advanced-topics/diffs).


Now let's put that change in the staging area
and see what `git diff` reports:

```bash
$ git add sundanese.txt
$ git diff
```
<!-- As far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory. -->
It displays no difference, because the new changes have been added to the staging area. To show differences between the last commit and what's staged, we need to specify the `--staged` flag:

```bash
$ git diff --staged
diff --git a/sundanese.txt b/sundanese.txt
index 315bf3a..b36abfd 100644
--- a/sundanese.txt
+++ b/sundanese.txt
@@ -1,2 +1,3 @@
 belongs to the Austronesian language family
 spoken in western Indonesia
+word order: SVO
```

Let's then save our changes through committing:

```bash
$ git commit -m "add word order info"
[master 005937f] add word order info
 1 file changed, 1 insertion(+)
```

check our status:

```bash
$ git status
On branch master
nothing to commit, working directory clean
```

and look at the history of what we've done so far:

```bash
$ git log
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Henry Higgins <profhiggins@oxford.edu>
Date:   Thu Aug 22 10:14:07 2018 -0400

    add word order info

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Henry Higgins <profhiggins@oxford.edu>
Date:   Thu Aug 22 10:07:21 2018 -0400

    add region information

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Henry Higgins <profhiggins@oxford.edu>
Date:   Thu Aug 22 09:51:46 2018 -0400

    start notes on Sundanese
```


## Exploring History 

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `sundanese.txt`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `sundanese.txt`, adding yet another line which unfortunately contains misinformation:

```bash
$ nano sundanese.txt
$ cat sundanese.txt
belongs to the Austronesian language family
spoken in western Indonesia
word order: SVO
a close relative of Spanish
```

Now, let's see what we get.

```bash
$ git diff HEAD sundanese.txt
diff --git a/sundanese.txt b/sundanese.txt
index b36abfd..0848c8d 100644
--- a/sundanese.txt
+++ b/sundanese.txt
@@ -1,3 +1,4 @@
 belongs to the Austronesian language family
 spoken in western Indonesia
 word order: SVO
+a close relative of Spanish
```

which is the same as what you would get if you leave out `HEAD`.  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1` to refer to the commit one before `HEAD`.

```bash
$ git diff HEAD~1 sundanese.txt
```

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:


```bash
$ git diff HEAD~2 sundanese.txt
diff --git a/sundanese.txt b/sundanese.txt
index df0654a..b36abfd 100644
--- a/sundanese.txt
+++ b/sundanese.txt
@@ -1 +1,4 @@
 belongs to the Austronesian language family
+spoken in western Indonesia
+word order: SVO
+a close relative of Spanish
```

We could also use `git show` which shows us what changes we made at an older commit as well as the commit message, rather than the _differences_ between a commit and our working directory that we see by using `git diff`.

```bash
$ git show HEAD~2 sundanese.txt
commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Henry Higgins <profhiggins@oxford.edu>
Date:   Thu Aug 22 10:07:21 2013 -0400

    start notes on Sundanese

diff --git a/sundanese.txt b/sundanese.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/sundanese.txt
@@ -0,0 +1 @@
+belongs to the Austronesian language family
```

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.


Alright! So
we can save changes to files and see what we've changed—now how
can we restore older versions of things? We need that, 
as we realize Sundanese is in fact not related to Spanish and decide to scrap that line. 
Checking `git status` tells us that the file has been changed,
but those changes haven't been staged:

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   sundanese.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back to the state of last commit 
by simply using `git checkout HEAD filename`:

```bash
$ git checkout HEAD sundanese.txt
$ cat sundanese.txt
belongs to the Austronesian language family
spoken in western Indonesia
word order: SVO
```

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead:

```bash
$ git checkout f22b25e sundanese.txt
$ cat sundanese.txt
belongs to the Austronesian language family
```


```bash
$ git status
# On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   sundanese.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```


Notice that the changes are on the staging area. 
If you decide to stick to this restored version, you will need to complete the process by committing, when the restored version will become the new `HEAD`. 
If not, you can go back to the last commit point using `git checkout HEAD filename`:

```bash
$ git checkout HEAD sundanese.txt
```



## Ignoring Things


What if we have files that we do not want Git to track for us,
like backup files created by our editor
or intermediate files created during data analysis?
Let's create a few dummy files:

```bash
$ cd ..
$ mkdir analysis
$ cd analysis
$ mkdir results
$ touch a.dat/ b.dat c.dat results/a.out results/b.out
```

and see what Git says:

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.dat
	b.dat
	c.dat
	results/
nothing added to commit but untracked files present (use "git add" to track)
```

Putting these files under version control would be a waste of disk space.
What's worse,
having them all listed could distract us from changes that actually matter,
so let's tell Git to ignore them.

We do this by creating a file in the root directory of our project called `.gitignore`:

```bash
$ nano .gitignore
$ cat .gitignore
*.dat
results/
```

These patterns tell Git to ignore any file whose name ends in `.dat`
and everything in the `results` directory.
(If any of these files were already being tracked,
Git would continue to track them.)

Once we have created this file,
the output of `git status` is much cleaner:

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
nothing added to commit but untracked files present (use "git add" to track)
```

The only thing Git notices now is the newly-created `.gitignore` file.
You might think we wouldn't want to track it,
but everyone we're sharing our repository with will probably want to ignore
the same things that we're ignoring.
Let's add and commit `.gitignore`:

```bash
$ git add .gitignore
$ git commit -m "Ignore data files and the results folder."
$ git status
# On branch master
nothing to commit, working directory clean
```
Check out [all these templates](https://github.com/github/gitignore) for `.gitignore` files.