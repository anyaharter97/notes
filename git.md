# Git
* [Configuration](git.md#configuration)
* [Commands](git.md#commands)
  * [`commit`](git.md#commit)
  * [`checkout`](git.md#checkout)
  * [`format-patch`](git.md#format-patch)
  * [`rebase`](git.md#rebase)
  * [`send-email`](git.md#send-email)
  * [Miscellaneous Commands](git.md#miscellaneous-commands)
* [Sending Patches to Mailing List](git.md#sending-patches-to-mailing-list)
* [Incorporating Master Changes on Branch](git.md#incorporating-master-changes-on-branch)

## Configuration
`~/.gitconfig` contains global git configurations  
`~/.gitignore` contains global gitignore instructions which are the files to be untracked by git   
`<project-directory>/git/config` contains project specific git configurations  

## Commands

#### `commit`
`git commit -a` stages all changes (but not for untracked files) and asks you for a commit message  
`git commit -am "message"` commits changes using "message" as the commit message  
`git commit --amend` allows you to change the commit message for the most recent commit  
`git commit -a --amend` adds the most recent changes as part of the most recent commit  

#### `checkout`
`git checkout -b <name>` creates a new branch &lt;name&gt;  
`git checkout <name>` opens branch &lt;name&gt; if it exists  

#### `format-patch`
`git format-patch -1` creates a patch from the last commit  
`git format-patch -6 --cover-letter` creates a patch from the last 6 commits with a cover letter  
`-v2` creates a patch from the last commit and adds "v2" to the header  

#### `rebase`
Rebasing in interactive mode `-i` allows you to reorder, delete, edit and squash commits together  
`git rebase -i HEAD~3` allows you to rebase the top 3 commits  
`git rebase master` from inside a branch applies your commits on top of the master (pull the master before this)  

#### `send-email`
`git send-email <patch-file>` sends an email with the &lt;patch-file&gt; (interactively requests sendee)  
`git send-email *.patch` sends all patches (make sure all old patches are deleted)  
`--to <address>` sends patch email to &lt;address&gt;  
`--cc <address>` sends patch email and CCs &lt;address&gt;  

#### Miscellaneous Commands
`git add --patch` allows you to split commits  
`git branch-info` returns details of all branches on master  
`git pull` pulls all changes from whatever branch you are inside  

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