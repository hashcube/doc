# Getting Started with Git

## Intro

### Getting around

Even though the *git utility* has many features, only a few
commands are needed to start working. And since it's
a very popular revision control system, there's plenty of
information about it, and *GitHub*---the popular
web-based hosting service we use to manage our git projects.

If you have tab-completion set up in your shell, list
available git commands with:

`$ git <tab>`

To see a git command's usage:

`$ git command -h`

And to see a git command's manual page:

`$ git help command`<br/>
or<br/>
`$ man git-command`

To see the differences between the working branch and the
last commit, and to display a list of additional commands
needed to unstage a file:

`$ git status`

To list branches, or to list *all* branches (including remotes):

`$ git branch`<br/>
`$ git branch -a`

To delete a remote branch (after, say, your feature request
has been merged):

`$ git push origin --delete branchname`

Please read the GitHub help for [managing remote branches](http://help.github.com/remotes/);
it's confusing to see a list of old branches that no one knows anything about.

Once a file has been committed, git may act funny when you
move it around. Even though git normally resolves discrepancies
on its own, it's worth understanding how these commands work:

`$ git mv file`<br/>
`$ git rm file`

#### See also:

* [git-scm.com/documentation](http://git-scm.com/documentation)
* [help.github.com](http://help.github.com)
* [gitref.org](http://gitref.org)


## Workflow
  
### A reasonable development model

The complications with git are not so much how to use it,
but how to use it when collaborating with other
people. Workflow is not something enforced by the tool, but
by the people using it. As such, check with your project leader
to make sure everyone is on the same page, obviously, that
direction supersedes any advice given here.

A popular development model is to reserve the `master` branch
for *production-ready* releases, and use `develop` as the
*integration branch*. When `develop` reaches a stable
point and is ready for release, the branch is merged back into `master`
and tagged with a release number. Feature branches are
forked from `develop`; this is where most of the day-to-day
coding occurs. Hotfixes are forked from `master` and merged
back into the `master` *and* `develop` branches.

#### See also:

* [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)


### Adding a feature

Since branching is easy with git, generally, a new feature is
added by creating a new branch and merging it back.

~~~
# create a new myfeature branch from the current branch (probably master)
$ git checkout -b myfeature

# ...hack, hack, hack...
$ git add file
$ git commit -m "A message about the changes made."
# ...rinse, repeat...

# switch back to main branch (or where you want to merge to)
$ git checkout master
# update branch to include changes
$ git merge myfeature

# remove the feature branch
$ git branch -d myfeature
~~~


### Update a branch from remote

`$ git pull origin mybranch`


### Working with others

Now we'll look at a more real-life example of adding a new
feature to the *develop* branch. Remember, *origin*  is just
a name for a branch, there's nothing inherently special
about it other than the convention to point it at the
GitHub remote repository.

~~~
# create myfeature branch from the develop branch
$ git checkout -b myfeature develop

# ...hack, hack, hack...
$ git add file
$ git commit -m "A message about the changes made."
# ...rinse, repeat...

# switch back to develop
$ git checkout develop

# update from remote (this is basically a git pull)
$ git fetch origin
$ git merge origin/develop
# develop branch is now up-to-date

# switch back to feature branch
$ git checkout myfeature

# update feature branch
$ git merge develop
# ...handle merge conflicts manually or use 'git mergetool'...

# if long-standing feature, push feature branch to remote for safekeeping
$ git push origin myfeature

# and optionally, to remove the feature branch
$ git checkout develop
$ git branch -d myfeature

~~~

### Checking out a remote branch

List remote branches and tracking status:

`$ git remote show origin`

If the remote branch is not listed as `tracked`, then fetch them:

`$ git fetch`

Now checkout the remote branch just like a local branch:

`$ git checkout -b mybranchname origin/branchname`


#### See also:

* [Remotes](http://help.github.com/remotes/)
* [Basic Merge Conflicts](http://git-scm.com/book/en/Git-Branching-Basic-Branching-and-Merging#Basic-Merge-Conflicts)


### Pull requests on GitHub

While the git tool has its own command for code management,
`git request-pull`, this process is quite a bit easier using
GitHub's *pull request* mechanism.

~~~
# create a new feature branch
git checkout -b myfeature

# ...hack, hack, hack...
git add file
git commit -m "A message about the changes made."
# ...rinse, repeat...

# push to github
git push origin myfeature
~~~

To issue a *pull request* on GitHub, navigate to the
repository page---make sure you're on the feature branch you
wish to merge---and click __Pull Request__.

Make sure to:

1. Enter a meaningful message about your changes.
2. Check the repository:branch *from* and *to* (to change these, click __Change Commits__).
3. Preview.
4. Click __Send Pull Request__

After your pull request has been accepted and merged, delete
the feature branch from the remote to cut down the clutter:

`$ git push origin --delete myfeature`


#### See also:

* [Send Pull Requests](http://help.github.com/send-pull-requests/)


## Git Tips

### Add sections of a file

Running `git add -p <file>` will let you interactively
choose which parts or the file to add to a commit. This is
can be helpful when you want to break up multiple additions
into separate commits, or for testing purposes where you
want to ignore some isolated changes.

### Merge sections of a file from another branch

While on the branch you want to merge *to*, this command
will interactively ask you to merge each chunk:

`git checkout --patch frombranch file`

### View history

List commits in reverse chronological order:

`$ git log`

List commits and include the diff with each one:

`$ git log -p`

Print each commit on a single line:

`$ git log --pretty=oneline`

Show the commit history of the file:

`$ git blame <file>`

Restrict history of a file to line numbers 20-25:

`$ git blame -L 20,25 <file>`

#### See also:

* [Viewing the Commit History](http://git-scm.com/book/en/Git-Basics-Viewing-the-Commit-History)


### See changes

To see what's been changed but not yet staged:

`$ git diff`

To see what's been staged and will go into the next commit:

`$ git diff --staged`

List the names of files changed in this branch against another:

`$ git diff --name-status anotherbranch`

View the changes between two arbitrary commits:

`$ git diff <commit> <commit>`

If only passed a single commit then it has the same effect as using HEAD instead.


#### See also:

* [Viewing Your Staged and Unstaged Changes](http://git-scm.com/book/en/Git-Basics-Recording-Changes-to-the-Repository#Viewing-Your-Staged-and-Unstaged-Changes)


### Fixing mistakes

If you've messed up the working tree, but haven't committed
yet, return to the last committed state with:

`$ git reset --hard HEAD`

If you've committed something you want to undo, you can
create a new commit that undoes whatever was done by the old
commit. To do this pass a reference of the bad commit to
`git revert`. For example, to revert the most recent commit:

`$ git revert HEAD`

#### See also:

* [Undoing Things](http://git-scm.com/book/en/Git-Basics-Undoing-Things)


### Copy a file from between branches

First, switch to the destination branch:

`$ git checkout destbranch`

Then, checkout the file you want from another branch:

`$ git checkout anotherbranch file`


### New tags

If there's a bug in a release, you'll need to release a new
tag.  But, if you update an existing tag, only people who
haven't seen the tag before will correctly get the tag.

If you do end up in this state, you can get the new tag by running:

~~~
git tag -d tagname
git fetch --tags
git checkout tagname
git reset --hard
~~~

### Ignore files

Often you won't want to track certain files or directories
in a git repository, such as log and build files. To ignore
these, create a list of matching file name patterns in a
`.gitignore` file in a directory of the git tree. You may
also create a global ignore file as well.

#### See also:

* [Doc: Ignoring Files](http://git-scm.com/book/ch2-2.html#Ignoring-Files)
* [GitHub: Ignore Files](http://help.github.com/ignore-files/)


### GUI

There are a number of third-party utilites for visualizing
a git repo, but there's also a simple, built-in, tool as well: 

`$ git gui`


### Colors

Here is Billy's `~/.gitconfig`, featuring his famous recipe for git console colors:

~~~
[user]
  name = Johnny Game Closure
  email = you@gameclosure.com

[apply]
  whitespace = nowarn

[color]
  branch = auto
  diff = auto
  status = auto

[color "branch"]
  current = yellow bold
  local = cyan bold
  remote = green bold

[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red bold
  new = green bold

[color "status"]
  added = yellow bold
  changed = green bold
  untracked = cyan
~~~


### Healthy repo

General repository cleanup helps reduce disk space and
increase performance by compressing file revisions and
removing unnecessary files and unreachable objects. `git gc`
can be run very quickly and is often automatically called by
some git commands. For a more aggressive, but
time-consuming, optimization, run:

`$ git gc --aggressive`
