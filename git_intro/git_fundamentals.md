# Git Fundamentals
This section covers the basics of setting up the git environment and the most
commonly used commands.

* Install the latest version of git for your computer: https://www.git-scm.com/downloads
  * Simply click on the link for your operating system to download the installer
  * Follow the instructions to install the package
  * Optionally install GitHub's desktop GUI client: https://desktop.github.com/
* If you don't already have one, sign up for a free GitHub account: https://www.github.com

Commands are executed in the terminal window. Examples in this document show the command
to be executed after a dollar sign ($) prompt. *Do not type the dollar sign when entering
commands.* Note that some commands include quotes around words or phrases. These quotes
are important -- *don't skip them*. Lines that begin with the hash symbol (#) are comments
for informational purposes only.

**Important:** git provides helpful information -- pay attention to the git output messages.

There are many git references and tutorials on-line. Here is the link to
[GitHub's bootcamp](https://help.github.com/categories/bootcamp/).

<!-- TOC -->

- [Git Fundamentals](#git-fundamentals)
- [Understanding git](#understanding-git)
- [Important set-up for the new git user](#important-set-up-for-the-new-git-user)
    - [Username & email](#username--email)
    - [Default editor](#default-editor)
- [Creating and contributing to a repo](#creating-and-contributing-to-a-repo)
    - [Git init](#git-init)
    - [Git clone](#git-clone)
    - [Git fork](#git-fork)
    - [GitHub pull request](#github-pull-request)
    - [Git commit messages](#git-commit-messages)
    - [Merge conflicts](#merge-conflicts)
    - [Git log and git show](#git-log-and-git-show)
- [Basic repo configuration](#basic-repo-configuration)
    - [Git remotes](#git-remotes)
    - [.gitignore](#gitignore)
- [Git Commands](#git-commands)
    - [git help](#git-help)
    - [git init](#git-init)
    - [git status](#git-status)
    - [git pull vs git fetch && git merge](#git-pull-vs-git-fetch--git-merge)
        - [What is git checkout? And a branch?](#what-is-git-checkout-and-a-branch)
    - [git add and git commit](#git-add-and-git-commit)
- [push changes to your online repo](#push-changes-to-your-online-repo)

<!-- /TOC -->

# Understanding git
Git is a _version control system_. It keeps track of changes that the
developer makes to the files in the repo, and allows these changes to
be shared with others, removed at a later time, or re-ordered and
combined to package several small changes into one.

A developer causes a change by editing files in the repo, or adding or
files from the repo. After editing the files, git must be told what the
change is so that it can track it. To do so, the developer groups the
changes together into a _commit_. The commit is the basic trackable
unit of git, and it is identified by a _hash_, a string of hexadecimal
numbers that uniquely identifies that particular change to the repo.

In order to create the commit, the developer will _add_ the files and
other changes to a _staging area_, where git keeps track of items that
will be included in the next commit. In this way, changes can be grouped
together into a batch that is included in a single commit.

All of the behind-the-scenes work performed by git goes on in a special
directory in the top directory of the repository: _.git_. This is the
place where changes are staged, and the rest of the repository history
is kept. Basically, the state of every file for the life of the repo is
instantly available to the developer through git commands that act on
the .git directory. (Note that the dot in front of the directory name
causes it to be hidden in a normal _ls_ directory listing. Use _ls -a_
to reveal these hidden files.)

![Git staging](images/git_staging.png)

# Important set-up for the new git user
## Username & email
When a developer commits a change to a repository, the commit is labeled
with their user name and email address. It is important to set up your
development machine with valid identity information so that your teammates
know who is working on what. While you can set up any name/address you
like, for the purposes of our team please use your real name and email
address.

You only need to set this up once when you install git on your computer.
After that, all repositories that you work in will be aware of your
identity. The information that you are entering here will be stored
in the _.gitconfig_ file in your user home directory.

```bash
# set username & email in global git configuration
$ git config --global user.name "Your Real Name"
$ git config --global user.email "your.real.email@somewhere.com"
```

## Default editor
Git brings up a text editor whenever the user is required to enter
information about a commit. The default editor that it chooses may
not be what the developer likes. It is possible to change the default
editor in a similar way to setting the user identity.

```bash
# set nano as the default editor in global git configuration
$ git config --global core.editor "nano -w"
```

# Creating and contributing to a repo
## Git init
You can create a repo easily on your computer. Doing so will create
a private repo, as it is not automatically associated with a server
(ie, GitHub) that will allow it to be shared. You can create these
repos wherever you like, but be aware that creating a repo inside
another repo can be confusing to work in. Creating and using repos
from scratch is a great way to try out git without fear of messing
up a shared group repo.

If you are creating a brand new repo, create your working directory
first, and switch to that directory before initializing the repo.

```sh
# create your working directory to host your project
$ cd
$ mkdir my_coolest_project
$ cd my_coolest_project

# initialize git repo
$ git init
Initialized empty Git repository in /Users/binnur/my_coolest_project/.git/
```

Initializing a git repo automatically creates a .git directory to manage
tracked files.  Deleting this directory will delete all project history.
__Don't do that unless you really want to destroy your copy of the repository__
```sh
# list all files
$ ls -a
```

At this point, our project tracking is setup and ready to go, but there are
no files in the repo. Remember previous comment about how helpful git is?
Let's take a look at its status after _git init_.
It tells us to use _git add_ to track files we want to add to version control.
```sh
# show the status of git
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

## Git clone
_git clone_ is the process of creating a copy of a shared remote repository.
The command takes a parameter that is the location of the remote repo,
and this location can be another git directory on the same machine, or
a URL that points to a remote network location.

With GitHub you can simply copy the URL for the repo and execute a _git clone_
command on your system to create a copy.

GitHub provides HTTPS or SSH URLs for cloning. HTTPS is the easiest way to
clone a public repository that you don't intend to push changes to. SSH uses
the _secure shell_ protocol to create an authenticated two-way link between
your computer and GitHub, and requires that you set up shared keys on your
machine and GitHub.
You can read more about this [here](https://help.github.com/articles/which-remote-url-should-i-use/)

![GitHub clone](images/clone.png)

```sh
# Clone the Atlas repository into your current directory
$ git clone https://github.com/Spartronics4915/Atlas.git
Cloning into 'Atlas'...
remote: Enumerating objects: 1144, done.
remote: Total 1144 (delta 0), reused 0 (delta 0), pack-reused 1144
Receiving objects: 100% (1144/1144), 219.01 KiB | 2.28 MiB/s, done.
Resolving deltas: 100% (461/461), done.
```

## Git fork
GitHub supports the use of the _forking workflow_. In this workflow, the
developer duplicates the main project repo into their own GitHub account
workspace, and then clones that duplicated repo onto their computer in
order to make changes to the project. In this way, the developer can
make all the changes that they want to the clone on their machine, and
can also push those changes to their copy of the repo on GitHub, without
affecting the main project at all.


Here are example of forks in my GitHub account.
![Binnur's forks](images/forks.png)

You can _fork_ any of the repositories you wish to copy, simply by clicking
on the "fork" button.
![Forking in GitHub](images/forking.png)

Once you have a fork in your account, you can _git clone_ it to your system.
Any changes you make to your forked repo will not impact the main project repo.
When you are satisfied that you have
a change that should be included in the main project, create a
_pull request_ to allow the team to review the changes and then _merge_
them from your repo into the main team repo. If issues are found,
the reviewer can make suggestions for changes to be made before merging.

## GitHub pull request
You can contribute to a forked repository by submitting a _pull request_ to the
upstream repository.

[See GitHub documentation](https://help.github.com/articles/creating-a-pull-request-from-a-fork/).

## Git commit messages
_git commit_ messages are a great way to share the changes that has been made to the repo and why. Writing good commit messages important for good collaboration.

What is a bad commit message? Check out examples from [here](https://www.codelord.net/2015/03/16/bad-commit-messages-hall-of-shame/).

Commit structure is as follows. Read more about writing good commits [here](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).
* 1st line is a summary, 50 chars or less
* 2nd line is a blank line
* 3rd+ lines are more detailed information about the change

## Merge conflicts
In a collaborative environment, merge conflicts are inevitable. Merge conflict occurs when your changes conflict with the base branch/repo. With that, git requires you to decide which changes are valid for the final merge.

Frequently staying in sync w/ your upstream/main repo helps avoid potential merge conflicts. This requires frequent _git pull_ from the upstream to ensure your work is in sync with main repo.

## Git log and git show
_git log_ is a great way to explore contribution to a repo. Some handy git log options.

```sh
# basic git log
$ git log
# one line summary of commits
$ git log --oneline
# commit statistics
$ git log --stat
# commits by author
$ git shortlog
# pretty the output
$ git log --pretty="%cn committed %h on %cd"
# commits for an author
$ git log --author="Tarkan"
```

_git show_ displays specific information on a given commit. As input, you provide the sha slice.

```sh
$ git show 97a7c5f0d
$ git show 97a7c5f0d --stat
```

# Basic repo configuration

## Git remotes
For background on _git remotes_, see [What is a repo and remotes?](./git_about.md#"What-is-a-repo-and-remotes").

```sh
# specify your URL for the upstream
$ git remote add upstream https://github.com/Spartronics4915/2017-STEAMworks
# show remotes for my local developers_handbook repo
$ git remote -v
binnur	git@github.com:binnur/developers_handbook.git (fetch)
binnur	git@github.com:binnur/developers_handbook.git (push)
origin	git@github.com:Spartronics4915/developers_handbook.git (fetch)
origin	git@github.com:Spartronics4915/developers_handbook.git (push)
```

Using the remotes, I can pull the updates from _origin_, i.e. Spartronics' repo
for developers_handbook, to stay in sync with other developers on the repo.
Using _binnur_ I can push my updates to my GitHub repo and submit a pull request to Spartronics.

Note, your remotes can be named to anything.
However, we will use the standard 'upstream' to refer to the source,
and 'origin' to refer to our fork.

## .gitignore
_.gitignore_ specifies all files that should not be tracked by git.
See [GitHub's ignoring files for more information] (https://help.github.com/articles/ignoring-files/).

```sh
# ignore sketch files, anywhere in the git repo
$ cat .gitignore
*.sketch
```

# Git Commands
## git help
You can access git help at anytime -- and don't forget to pay attention to git prompts after git commands are applied!

```sh
# git help
$ git help
# git help on status command
$ git help status
# git help on clone command
$ git help clone
```

## git init
See [git init](./#Git-init) section for more information.

```sh
# initialize git repo
$ git init
Initialized empty Git repository in /Users/binnur/my_coolest_project/.git/
```

## git status
_git status_ is your friend! It is an overview of the status of the repo.

```sh
$ git status
On branch gitintro
Your branch is ahead of 'binnur/gitintro' by 1 commit.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	git_intro/git_advanced.md
	git_intro/git_configure.md
	git_intro/git_faq.md
	git_intro/git_fundamentals.md
	git_intro/images/.DS_Store
	git_intro/images/clone.png
	git_intro/images/forking.png
	git_intro/images/forks.png
	git_intro/images/git_staging.png
	robot_lessons/.vscode/
	robot_lessons/.wpilib/
	robot_lessons/lesson1.code-workspace
	robot_lessons/lesson1/.settings/org.eclipse.jdt.core.prefs
	robot_lessons/lesson1/build/
	tutorials.code-workspace

nothing added to commit but untracked files present (use "git add" to track)
```
Looking at the git status, I can tell:
* I am on 'gitintro' branch
* My branch is ahead by 1 commit, meaning other developers following my fork on binnur/gitintro would not get my local updated
* I have several untracked files and I need to use _git add_ to track, i.e. add them to the repo

## git pull vs git fetch && git merge
_git pull_ vs _git fetch && _git merge_ has a similar outcome with different
intents.

_git pull_ pulls the changes from the remote and magically applies them to your
local repo. It is a _fetch && merge_ in one. The _git pull_ fetches and
downloads content from a remote repository and immediately updates the local
repository to match that content.

```sh
# by default fetches from the remote your local repo is tracking and applies to your current branch
$ git pull
```

However, as it is magical, recommendation is to use _fetch_ and _merge_ to
ensure you understand how your working directory will be updated, this is
important when you get to next level of git with branches.

_git fetch_ downloads all related commits, files, etc. from remote repo to your local repo. It allows you to see what everyone has been working on. As it does not apply the changes to your local repo, _git fetch_ allows you to review changes and deciding how to _git merge_ the updates.

```sh
# fetch all content
$ git fetch --al
# fetch specific remote & branch
$ git fetch upstream master
# dry-run fetch command
$ git fetch --dry-run <remote_name>
```

_git merge_ will apply the changes to your local repo after _git fetch_.

```sh
# merge origin/master
$ git merge origin/master
# merge all of origin's content
$ git merge origin
```

Merge process can result in conflicts -- this is basically git's way of saying
it cannot automatically decide how to handle conflicts and it needs help to
determine which version/entry is the correct one. Read more on [handling merge
conflicts](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/).

### What is git checkout? And a branch?
When you perform _git pull_, git automatically checkout the default branch that
is set by the remote repo. _git checkout_ allows the developer to switch between different versions of a branch or commits. This process updates the contents of your working directory to the state of that branch or commit.

In general, you may checkout between branches, such as creating a feature branch to ensure your master branch is production ready at all times.

```sh
# create and checkout a new branch at the same time
$ git checkout -b my-new-branch
# above command w/ '-b' is same as:
$ git branch my-new-branch
$ git checkout my-new-branch
# checkout master branch
$ git checkout master
# checkout a specific branch
$ git checkout <branch-name>
# print a list of branches
$ git branch -a
```

## git add and git commit
_git add_ and _git commit_ are used in combination to 'save' git repo's current state. _git add_ makes a change to the staging area by adding files from working directory to repo's staging area. Changes are not recorded until you _git commit_.

As discussed in the [git status](./#git-status) section, _git status_ highlights the status of the repo.

```sh
# start tracking the git_fundamentals.md file:w
$ git add git_intro/git_fundamentals.md
# add every file in the current directory
$ git add .
# add every file in the specified directory
$ git add <dir name>
```

_git commit_ will publish the git_intro/git_fundamentals.md file to the local repo

```sh
$ git commit
[gitintro c0bbf03] Initial content to 'git' going
 1 file changed, 325 insertions(+)
 create mode 100644 git_intro/git_fundamentals.md
 ```

## git push
Using _git push_ commits changes made to your local repo to remote repository. _git push_ takes two arguments:
* remote name, ex. _origin_
* branch name, ex. _master_

```sh
# push changes to your online repo
$ git push origin master
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 4 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (9/9), 447.26 KiB | 20.33 MiB/s, done.
Total 9 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:binnur/developers_handbook.git
   30d5926..71bda4d  gitintro -> gitintro
```
### What is "non-fast-forward"?
When your local repo is behind the repo you are pushing to you will see an error
`non-fast-forward updates were rejected`. That means you need to retrieve the
changes before you can push your changes. Read more about [git pull vs git fetch
&& git merge](#git-pull-vs-git-fetch-&&-git-merge).
