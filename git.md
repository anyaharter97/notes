# Git
* [Configuration](git.md#configuration)
* [Commands](git.md#commands)
    * [`add`](git.md#add)
    * [`apply`](git.md#apply)
    * [`bisect`](git.md#bisect)
    * [`blame`](git.md#blame)
    * [`branch`](git.md#branch)
    * [`branch-info`](git.mid#branch-info)
    * [`bundle`](git.md#bundle)
    * [`clean`](git.md#clean)
    * [`clone`](git.md#clone)
    * [`commit`](git.md#commit)
    * [`checkout`](git.md#checkout)
    * [`diff`](git.md#diff)
    * [`format-patch`](git.md#format-patch)
    * [`grep`](git.md#grep)
    * [`log`](git.md#log)
    * [`ls-files`](git.md#ls-files)
    * [`merge`](git.md#merge)
    * [`pull`](git.md#pull)
    * [`rebase`](git.md#rebase)
    * [`reflog`](git.md#reflog)
    * [`remote`](git.md#remote)
    * [`reset`](git.md#reset)
    * [`send-email`](git.md#send-email)
    * [`show`](git.md#show)
    * [`stash`](git.md#stash)
* [`git add --patch` More Info](git.md#git-add---patch-more-info)
* [Sending Patches to Mailing List](git.md#sending-patches-to-mailing-list)
* [Incorporating Master Changes on Branch](git.md#incorporating-master-changes-on-branch)
* [Merging Branch into Master](git.md#merging-branch-into-master)
* [Splitting Commits](git.md#splitting-commits)
* [Adding a Remote](git.md#adding-a-remote)
* [Adding a Remote Branch](git.md#adding-a-remote-branch)
* [Remote Mirror Repository](git.md#remote-mirror-repository)
    * [Initial Set Up on Local (Create Bundle)](git.md#initial-set-up-on-local-create-bundle)
    * [Initial Set Up on Air-Gap (Install Bundle)](git.md#initial-set-up-on-air-gap-install-bundle)
    * [Creating Delta Bundle on Local](git.md#creating-delta-bundle-on-local)
    * [Importing Delta Bundle on Air-Gap](git.md#importing-delta-bundle-on-air-gap)
    * [Moving Changes Back to Local](git.md#moving-changes-back-to-local)

## Configuration
`~/.gitconfig` contains global git configurations  
`~/.gitignore` contains global gitignore instructions which are the files to be untracked by git   
`<project-directory>/git/config` contains project specific git configurations  

## Commands

#### `add`
* `git add <filename>` stages the changes for that filename  
* `git add .` stages all changes  
* `git add --patch` allows you to stage changes by hunk rather than by file ([more info](git.md#git-add---patch-more-info))
* `git add -N <filename>` adds the new file to "Changes not staged for commit"

#### `apply`
* `git apply <patch>` applies the patch file to the current branch  
* `git apply --reverse <patch>` undoes the application of the patch file to that branch

#### `bisect`
1. `git bisect start` start git bisect
2. Give a good and bad commit as a starting point
    * `git bisect bad <commit number>` instance of a bad commit (omitting commit is current commit)
    * `git bisect good <commit number>` instance of a good commit (omitting commit is current commit)
3. Then it drops you halfway between the good and bad commits
4. Run some diagnostic to tell if the commit is good or bad (git grep, make check, test bug etc.)  
5. Say the status of the commit
    * `git bisect bad` if current commit is bad
    * `git biesct good` if current commit is good
6. Repeat steps 4. and 5. until it shows the commit message
7. `git bisect reset` return to the commit checked out before the git bisect start

#### `blame`
* `git blame <filename>` shows the commits responsible for the current state of the file, line by line

#### `branch`
* `git branch` lists the branches
* `git branch -a` lists all of the branches including remotes
* `git branch <branch-name>` creates a new branch named &lt;branch-name&gt; from the current branch but does not switch to new branch  
* `git branch -D <branch-name>` deletes branch  
* `git branch -m <old-name> <new-name>` renames branch &lt;old-name&gt; to &lt;new-name&gt; (inside a branch, the &lt;old-name&gt; argument is unnecessary)

#### `branch-info`
* `git branch-info` returns formatted details of most recent commit for each branch (alias configured in gitconfig)

    ```
    [alias]
        branch-info = "!sh -c ' \
    git branch --list --no-color | sed -e \"s/*/ /\" | \
    while read branch; do \
        fmt=\"%Cred$branch:%Cblue %s %Cgreen%h%Creset (%ar)\"; \
        git --no-pager log -1 --format=format:\"$fmt\" $branch -- ; echo;\
    done'"
    ```

#### `bundle`
* `git bundle create <reponame>.bundle --all` creates a standalone image of the repo with all branches that can be [`clone`](git.md#clone)d elsewhere

#### `clean`
* `git clean -n` shows which files would be deleted by the `git clean` command  
* `git clean -f` forces the command through (deletes untracked files)  
* `git clean -d` deletes directories  
* `git clean -X` deletes ignored files  

#### `clone`
* `git clone /path/to/.bundle` creates a local version of the repo using the image from the bundle  
* `git clean http://github.com/project/link` creates a local version of the repo linked to the remote repo at that link
    * The link can be attained by going to the project on github and clicking the "Clone or download" button

#### `commit`
* `git commit -a` stages all changes (but not for untracked files) and asks you for a commit message  
* `git commit -am "message"` commits changes using "message" as the commit message  
* `git commit --amend` allows you to change the commit message for the most recent commit  
* `git commit -a --amend` adds the most recent changes as part of the most recent commit  
* `git commit --signoff` adds the signoff message at the bottom of the commit

#### `checkout`
* `git checkout -b <name>` creates a new branch &lt;name&gt;  
* `git checkout <name>` opens branch &lt;name&gt; if it exists  

#### `diff`
* `git diff` shows the changes made since the most recent commit  
* `git diff commithash1..commithash2` shows the changes made between the two commits 

#### `format-patch`
* `git format-patch -1` creates a patch from the last commit  
* `git format-patch -6 --cover-letter` creates a patch from the last 6 commits with a cover letter  
* `-v2` creates a patch from the last commit and adds "v2" to the header  

#### `grep`
* `git grep <string>` searches for the string recursively (case sensitive)
* `git grep -A4 <string>` displays 4 lines of context after line with string
* `git grep -B4 <string>` displays 4 lines of context before line with string

#### `log`
* `git log` show the commit log accessible from refs (ie. heads, tags, remotes)

#### `ls-files`
* `git ls-files` shows all files tracked by git in the directory the command is run from

#### `merge`
* `git merge <branch-a>` run from branch-b to merge the changes from branch-a onto branch-b

#### `pull`
* `git pull` pulls all changes down including newly created remote branches
* `git pull origin <remote-branch>` pulls down all changes from that branch
* `git pull origin master` pulls down only changes from the master branch
* `git pull /path/to/.git <branchname>` pulls changes to the branch specified from a .git file

#### `rebase`
* `git rebase -i HEAD~3` allows you to rebase the top 3 commits  
    * Rebasing in interactive mode `-i` allows you to reorder, delete, edit and squash commits together  
* !! better to use merge `git rebase master` from inside a branch applies your commits on top of the master (pull the master before this)  
* `git rebase --abort` quits out of a rebase
* `git rebase --continue` continues the next command in the rebase after you are done making changes
* `git rebase -x "make && make check" origin/master` rebases the current branch on top of origin master and calls "make && make check" for every commit that is applied on top of master

#### `reflog`
* `git reflog` shows all commits that are or were referenced in your local repo at any time, shows actions such as checkouts not visible with [`git log`](git.md#log) command

#### `remote`
* `git remote show origin` shows the origin for that repo
* `git remote` shows the repos whose branches are tracked

#### `reset`
Tread with caution so you don't lose changes forever by accident
* `git reset --hard HEAD~3` trashes the last three changes forever
* `git reset --hard commithash` reverts back to the state at this commit (can be used to undo a merge -- use the [`reflog`](git.md#reflog) to find it)
* `git reset HEAD^` undoes the last committed change but leaves changes as uncommitted (basically undoes the commit but otherwise changes nothing)
* `git reset --soft HEAD~1` undoes the last committed change but leaves changes as uncommitted (basically undoes the commit but otherwise changes nothing)

#### `send-email`
* `git send-email <patch-file>` sends an email with the &lt;patch-file&gt; (interactively requests sendee)  
* `git send-email *.patch` sends all patches (make sure all old patches are deleted)  
* `--to <address>` sends patch email to &lt;address&gt;  
* `--cc <address>` sends patch email and CCs &lt;address&gt;  

#### `show`
* `git show` shows the changes made in the last commit  
* `git show <commit>` shows the commit data and diff  
* `git show <commit>~ <commit>` shows the difference between the commit's ancestor and the commit  

#### `stash`
* `git stash` adds changes to the stash
* `git stash --patch` allows you to select hunks to be stashed
* `git stash push -m "<message>"` allows you to add the changes to the stash with a message
* `git stash show` shows the details of the top stashed changes
* `git stash list` lists all of the stashed changes
* `git stash apply` takes the top stashed changes and revives them (leaves the changes in the stash)
* `git stash pop` takes the top stashed changes, revives them, and removes them from the stack (if there is a merge conflict, it is not removed)
* `git stash drop` removes the top stashed changes from the stash
* `git stash clear` empties the stash

## `git add --patch` More Info
* `y` stage this hunk
* `n` do not stage this hunk
* `q` quit; do not stage this hunk or any of the remaining ones
* `a` stage this hunk and all later hunks in the file
* `d` do not stage this hunk or any of the later hunks in the file
* `g` select a hunk to go to
* `/` search for a hunk matching the given regex
* `j` leave this hunk undecided, see next undecided hunk
* `J` leave this hunk undecided, see next hunk
* `K` leave this hunk undecided, see previous hunk
* `s` split the current hunk into smaller hunks
* `e` manually edit the current hunk
* `?` print help

## Sending Patches to Mailing List
1. Make sure the branch has all changes from master before sending patches and make sure all changes have been added and incorporated into the latest commit (or more if necessary)  
2. `git commit --amend` change the commit message to be a good one to be sent out  
3. Delete old patches first to make tab-complete easier and also so you don't mess up  
4. `git format-patch -1` or whatever is relevant (see below)  
5. Check the file created to make sure it has the right header etc.  
    * if sending multiple, add a subject line and blurb to cover letter and check all files
6. `git send-email <patch-file>` with appropriate arguments if desired (see below)  

## Incorporating Master Changes on Branch
1. `git checkout master` to move to master  
2. `git pull` from master to get all of the changes  
3. `git checkout <branch>` to go back to the branch  
4. `git merge master` to apply changes branch changes on top of master within branch  

## Merging Branch into Master
1. `git checkout -b <branch>` to create and checkout a new branch  
2. Make changes
3. `git commit -am "<commit message> on <branch>"` to commit your changes  
4. `git push origin <branch>` to push changes to the remote <branch> branch  
5. `git checkout master` to move to master  
6. `git merge <branch>` to merge the <branch> with master  
7. `git push origin master` to push changes to master up to remote master branch  
8. `git branch -d <branch>` to delete the <branch> when you're finished    

## Splitting Commits  
1. `git rebase -i HEAD~4` or however back you need to go
2. Mark the relevant commit as "edit"
3. `git reset HEAD^` to uncommit the changes
4. `git add --patch` to add some hunks and not others
5. `git commit` and add a message (be careful not to use the "-a" flag or it will add everything)
6. Repeat 4. and 5. until everything has been committed
7. `git rebase --continue`

## Adding a Remote
1. Fork the project on github
2. Get the link to the fork from clicking the "Clone" button
3. From inside the repo run `git remote add <name> <link>`
4. To push changes run `git push -f <name>`

## Adding a Remote Branch
1. From within the main repo and in master branch...
2. Run `git checkout -b <new-branch>` to create the new local branch
3. Then `git push origin <new-branch>` to create the remote branch

## Remote Mirror Repository
This is used when you want a copy of a repo on an air-gap laptop / sneakernet.  

__All work on remote repo should be confined to a single branch (or merged into this single branch to consolidate the import).  Having deltas on the master branch requires bundle cleaning which is messy.  After cloning the original repo to start the remote repo, DO NOT CHANGE MASTER.__  

The following sections detail set up and updates going from the live local version to the standalone mirror and back.  

### Initial Set Up on Local (Create Bundle)
1. Pick a location to create a local copy of the repo. From there, run `git clone <location-of-remote-source-repo>`
2. `cd <repo-name>` to go to the top-most directory within the repo and create a tag by running `git tag <tag-name>`
3. Push the tag up to remote by running `git push origin <tag-name>`
4. `cd ..` to go back outside repo
5. `git clone --mirror <location-of-remote-source-repo>` to create a bare metal copy of the source repo
6. Enter the directory created by running `cd <repo-name>.git`
7. Create a bundle by running `git bundle create <tag-name>.bundle --all`
    * You can also specify a destination path for the bundle such as `git bundle create /path/to/put/<tag-name>.bundle --all`
8. Burn this bundle to a disk or other transfer media

### Initial Set Up on Air-Gap (Install Bundle)
1. If multiple air-gaps can be connected, make a shared location called "shareme" and run the following commands from inside
2. On an air-gap laptop, copy the bundle off the disk (or other transfer media) to a "/shareme/bundles" folder
    * This folder is optional but sets up a good standard for keeping all bundles in one place
3. From inside "shareme", run `git clone --mirror /bundles/<tag-name>.bundle <repo-name>.git` to create the mirror bare repo
4. You have now created a parallel bare metal repo that can be cloned from any laptop with access to "shareme" using the `git clone /shareme/<repo-name>.git` command

### Creating Delta Bundle on Local
1. Make sure to `git pull` before starting to ensure all the latest changes are present
2. Tag the current state by running `git tag <tag-name>` (make sure the tag-name is unique and informative as to the significance of this state)
3. To create a bundle of deltas since the previous tag, run `git bundle create <tag-name>.bundle --branches --tags ^<previous-tag-name> <tag-name>`
    * You can also specify a destination path for the bundle such as `git bundle create /path/to/put/<tag-name>.bundle`
4. Burn this bundle to a disk or other transfer media

### Importing Delta Bundle on Air-Gap
1. On air-gap laptop, copy the bundle off the disk (or other transfer media) to "/shareme/bundles" folder
2. From inside the repo, run `git pull /shareme/bundles/<tag-name>.bundle refs/heads/master master` to apply changes from the master branch in the bundle to the local master branch
3. You can then push these changes to the air-gap remote stored in /shareme/ to be seen by other air-gaps on the closed network
4. Remember to restrict all changes made in this environment to a single branch

### Moving Changes Back to Local
1. From the air-gap, zip up the bare repo using the following command (run from "shareme") `tar czf <desired-name>.tar.gz <repo-name>.git`
2. Burn the tar-gz file to a disk or other transfer media
3. Copy the tar-gz file locally and unzip (`tar xvzf <desired-name>.tar.gz`), revealing the `<repo-name>.git` directory
4. Run `git pull /path/to/.git <branch-name>` to grab changes from the development branch in the air-gapped environment records

