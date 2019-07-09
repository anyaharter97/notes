# Git
* [Configuration](git.md#configuration)
* [Commands](git.md#commands)
    * [`add`](git.md#add)
    * [`apply`](git.md#apply)
    * [`bisect`](git.md#bisect)
    * [`blame`](git.md#blame)
    * [`branch`](git.md#branch)
    * [`branch-info`](git.mid#branch-info)
    * [`clean`](git.md#clean)
    * [`commit`](git.md#commit)
    * [`checkout`](git.md#checkout)
    * [`diff`](git.md#diff)
    * [`format-patch`](git.md#format-patch)
    * [`grep`](git.md#grep)
    * [`log`](git.md#log)
    * [`pull`](git.md#pull)
    * [`rebase`](git.md#rebase)
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
* `git branch` lists all of the branches
* `git branch <branch-name>` creates a new branch named &lt;branch-name&gt; from the current branch but does not switch to new branch  
* `git branch -D <branch-name>` deletes branch  
* `git branch -m <old-name> <new-name>` renames branch &lt;old-name&gt; to &lt;new-name&gt; (inside a branch, the &lt;old-name&gt; argument is unnecessary)

#### `branch-info`
* `git branch-info` returns details of all branches on master (alias configured in gitconfig)

    ```
    [alias]
        branch-info = "!sh -c ' \
    git branch --list --no-color | sed -e \"s/*/ /\" | \
    while read branch; do \
        fmt=\"%Cred$branch:%Cblue %s %Cgreen%h%Creset (%ar)\"; \
        git --no-pager log -1 --format=format:\"$fmt\" $branch -- ; echo;\
    done'"
    ```

#### `clean`
* `git clean -n` shows which files would be deleted by the `git clean` command  
* `git clean -f` forces the command through (deletes untracked files)  
* `git clean -d` deletes directories  
* `git clean -X` deletes ignored files  

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

#### `format-patch`
* `git format-patch -1` creates a patch from the last commit  
* `git format-patch -6 --cover-letter` creates a patch from the last 6 commits with a cover letter  
* `-v2` creates a patch from the last commit and adds "v2" to the header  

#### `grep`
* `git grep <string>` searches for the string recursively (case sensitive)
* `git grep -A4 <string>` displays 4 lines of context after line with string
* `git grep -B4 <string>` displays 4 lines of context before line with string

#### `log`
* `git log` show the git log
* `git log -S<string>` show commits where the number of occurrences change
* `git log --author <name>` returns commits authored by &lt;name&gt;

#### `pull`
* `git pull` pulls all changes down including newly created remote branches
* `git pull origin <remote-branch>` pulls down all changes from that branch
* `git pull origin master` pulls down only changes from the master branch

#### `rebase`
* `git rebase -i HEAD~3` allows you to rebase the top 3 commits  
    * Rebasing in interactive mode `-i` allows you to reorder, delete, edit and squash commits together  
* `git rebase master` from inside a branch applies your commits on top of the master (pull the master before this)  
* `git rebase --abort` quits out of a rebase
* `git rebase --continue` continues the next command in the rebase after you are done making changes
* `git rebase -x "make && make check" origin/master` rebases the current branch on top of origin master and calls "make && make check" for every commit that is applied on top of master

#### `remote`
* `git remote show origin` shows the origin for that repo
* `git remote` shows the repos whose branches are tracked

#### `reset`
Tread with caution so you don't lose changes forever by accident
* `git reset --hard HEAD~3` trashes the last three changes forever
* `git reset HEAD^` undoes the last committed change but leaves changes as uncommitted (basically undoes the commit but otherwise changes nothing)

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
4. `git rebase master` to apply changes branch changes on top of master within branch  

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
