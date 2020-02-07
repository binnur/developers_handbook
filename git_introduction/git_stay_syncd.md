# Tips: Stay In-Sync with Upstream
Git workflow and GitHub can be helpful in minimizing merge conflicts, but it
requires discipline. Best practices include:

1. Make sure your code builds before initiating PR -- *don't break the master*
2. Ensure you pulled the latest updates from upstream and merged into your code.
3. Life is grand when PRs are merged immediately. But, that doesn't always
   happen. When you notice your PR is stale and there are updates to the
   `upstream/master`, stay in sync by updating your PR.

Important note on the assumed setup in regards to `origin` vs. `upstream`. See
[Review remotes](#review-remotes) section to verify how your `remotes` are setup.

## If `master` is the working branch
It is a good practice to push to your `origin` after each new functionality,
defect fixing, or in-progress code at the end of the workday. Think GitHub as a backup for your code.


```sh
# make sure your changes are committed
$ git commit -a
# fetch the all the latest from upstream and do some housekeeping
$ git fetch --all -p
# review logs to see what changed
$ git log upstream/master
# use rebase to place the upstream changes before your updated code
$ git rebase upstream/master
# push updates to origin, i.e. your fork --> this will automatically update your PR
$ git push
```

### `git push --force`
Note: `git push` might require a forced push as you are updating the remote to
reflect the state of the local refs, i.e. changing the history of your previous
commit. However, `--force` can be destructive and should only be performed if
you are the only one working off your remote, in this case `origin/master`.

`$ git push -f # or --force`

## If working off a branch
Using branches is a general best practice for git workflows. It allows your
local `master` to stay in sync with your fork and team repo, and while using
branches for feature development and bug fixes. [See About branches](#about-branches)

General practice:
1. Keep local `master` sync'd with `upstream` and `origin`
2. Create new branches from sync'd local `master`
3. Keep branch sync'd with `upstream` prior to PR

```sh
# sync local `master` with upstream
$ git fetch upstream master     # fetch upstream/master
$ git log                       # review updates
$ git merge upstream/master     # merge upstream/master to local master
$ git push                      # push updates to origin/master, i.e. your fork
```

```sh
# create a new branch
$ git checkout -b <branch-name>     # -b creates the branch and checks it out
# do stuff on the branch & check in updates
$ git commit -a
# sync w/ upstream
$ git fetch upstream master         # get upstream updates
$ git rebase upstream/master        # insert upstream changes before your updates
$ git push                          # if remote tracking is not set, this will error
# if git push error, i.e. first time pushing this branch -- set remote tracking
$ git push -u origin branch-name
```

Using your browser, browse to your GitHub fork -- it will highlight the new
updates from your branch with option to initiate a PR to the team repo.

## If experience merge conflicts during rebase
```sh
# rebase will list out conflicting files
# -- review and resolve merge conflict, i.e. look for HEAD... in the file
# -- save and add file and continue with rebase
$ git add fileName
$ git rebase --continue
$ git push origin branch-name
```

## Review remotes
* `origin` is the name of a remote. `origin` points to the remote when `git clone`
  was used to create a local repository. For Spartronics workflow, `origin` is
  our fork from the team repo. Note, remotes can be any name, `origin`
  is a default name given by GitHub.
* `upstream` points to the team repo and added by using `git remote add upstream
  <url>`. Similarly, `upstream` is just a name we standardized on for team communication.

```sh
$ git remote -v
origin  git@github.com:binnur/2020-InfiniteRecharge.git (fetch)
origin  git@github.com:binnur/2020-InfiniteRecharge.git (push)
upstream        git@github.com:Spartronics4915/2020-InfiniteRecharge.git (fetch)
upstream        git@github.com:Spartronics4915/2020-InfiniteRecharge.git (push)
```

```sh
# how a remote is configured
(master) binnur@Moya(developers_handbook)> git remote show origin
* remote origin
  Fetch URL: git@github.com:binnur/developers_handbook.git
  Push  URL: git@github.com:binnur/developers_handbook.git
  HEAD branch: master
  Remote branches:
    binnur/bling  tracked
    gitintro      tracked
    master        tracked
    ...
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

## About branches
* `git branch -a` lists both remote-tracking branches and local branches.
* `master` is the name of the **default main branch** that git creates when repo
  is created. While `master` is your local branch, `origin/master` is a remote
  branch.

For more on branches, [see Branches and Tags](https://github.com/Spartronics4915/developers_handbook/blob/master/git_introduction/git_advanced.md#branches-and-tags).

```sh
# list of remote and local branches
(master) binnur@Moya(developers_handbook)> git branch -a
  binnur/tips-git-in-sync
* master
  remotes/origin/HEAD -> origin/master
```
