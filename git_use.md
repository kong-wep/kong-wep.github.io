# Git Professional Guide คู่มือ Git มืออาชีพ 

เขียนโดย Pakkapol

แปลไทยโดย ภัคพล

## Table of Contents
- [Command Summary](#0-command-summary)
- [What is Git](#1-what-is-git)
- [](#2-concepts)
- [](#3-terminology)
- [](#4-files)
- [All Summary](#5-all-summary)
- 
- [Git Status](#git-status)
- [Git Add/Remove](#git-addremove)
- [Git Restore](#git-restore)
- [Git Commit](#git-commit)
- [Git Log](#git-log)
- [Git Diff](#git-diff)
- [Git Branch](#git-branch)
- [Git Merge](#git-merge)
- [Git Pull](#git-pull)
- [Git Push](#git-push)
## 0. Command Summary
```sh
$ git status # status of dif

$ git add <filename> # add new changes in file
$ git add .          # add all changes

$ git rm <filename>  # remove file from git
$ git restore        # restore all staging
$ git restore --staged # restore working to staging

$ git commit -m "<message>" # Commit

$ git log            # show logs

$ git diff           # difference

$ git branch         # show branches
$ git branch <name>  # create new branch
$ git checkout <name/id> # go to branch/commit with name/id

$ git merge <branch> # merge branch with current branch

$ git clone <url>    # copy directory
$ git remote -v      # show remote servers
$ git remote add <repo> <url> # add origin

$ git push           # push to server
$ git pull           # pull from server

# Other

$ git stash          # save changes
$ git init           # init local repository
$ git init --bare    # init remote repository
$ git fetch          # fetch from server to local repo, merge later
```

```
https://medium.com/@chonglee30/git-overview-30618e81eb77

<Working Dir>  <Staging Area>      <Local repo>   <remote repo>
      |---- add ----->|                  |               |
      |<-- restore ---|                  |               |
      |               |<-restore staged--|               |
      |               |---- commit ----->|               |
      |               |                  |---- push ---->|
      |<--------------------- pull ----------------------|
      |               |<----- merge -----|<---- fetch ---|
```

## 1. What is Git
Version control system for text files, and a 'distributed database'

## 2. Concepts
1. Commits are a list of file edits
2. Git is a tree of commits
3. Commits are permanent, all edits are new commits
4. Branches, HEAD, Tag are markers on the tree

## 3. Terminology 
`Commit` = is a **permanent** record of edits, commit has `commit id`

`Working directory` = Local files 

`Staging Area` = Chosen changes for commit

`Local repository` = Commits saved locally

`Remote repository` = Commits saved remotely

`Branch` = Series of commits
```
       commits
       v    v
   .->[5]->[6]      <--- branch
   |
[1]-->[2]->[3]->[4] <--master branch
```
`HEAD` = Commit you are looking at (latest commit)
## 4. Files
All git files in `.git`

Files for git to ignore `.gitignore`

## 5. All Summary
```
** Summary **
1 project, many repos (local remote ...)
1 repo, many branches (master, branch_b, branch_a ...)
1 branch, many commits

Auction project
===============
 work,stage
    | |
    HEAD
     V                _________________
a b mas a b   master |         /       |
 \/ \/   \/ \/       |    [Commit d435]|
  \ /     \ /  <-----|       /         |
   |       |         | [Commit f4356]  |
   o       o         |     /           |
local     remote     |____/____________|

1. Edit files (vscode)
2. Choose edits (git add)
3. Commit chosen (git commit)
4. Sync with server (git pull, git push)
```

****

## Git Status
Show status of git

```sh
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
   (use "git restore --staged <file>..." to unstage)
      modified:   main.py      # staging

Changes not staged for commit:
   (use "git add <file>..." to update what will be committed)
   (use "git restore <file>..." to discard changes in working directory)
      modified:   entity.py    # working

Untracked files:
   (use "git add <file>..." to include in what will be committed)
      object.py                # working, new
```
## Git Add/Remove
Add/remove files to staging area
```sh
$ git add <filename...> # add multiple files
$ git add .             # add everything
```

## Git Restore
Restore changes in working dir
```sh
$ rm test.txt           # erased by mistake, not added
$ git status
Changes not staged for commit:
      deleted:   entity.py
$ git restore entity.py # back to before the delete
```
```sh
$ git add database.js    # modified database
$ cat "Fish" > test.txt  # modified other file
$ git add test.txt
...
# Want to commit database only
$ git restore --staged test.txt # remove from staging
$ git commit -m "Modified database" # commit only database
```

## Git Commit
Save changes permanently
```sh
$ git commit -m "<message>" # commit to local repo
[master (root-commit) a6f95ec] a
 1 file changed, 1 insertion(+)
 create mode 100644 a
```

## Git Log
Show log of all commits
```sh
commit 8fbf79d3a49754019b5e92606dfd36999a3b3fc6 (HEAD -> fuck, server/master, origin/master, origin/fuck, master)
Author: user <user.name@mail.com>
Date:   Mon Aug 29 18:40:51 2022 +0700

    Added readme     # commit message

commit f54147abc02fc28cb4bfee1b0d5a767858fcf23f
Author: user <user.name@mail.com>
Date:   Mon Aug 29 18:38:04 2022 +0700

    Added gitignore
```

## Git Diff
Show differences between commits

Default: difference between working and staging
```diff
$ git diff
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..0151261
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
 README
 # Frontend
 React Server
+Mongodb
-Mongodd
```

## Git Branch
List/Modify git branches
```sh
$ git branch    # show all branches in local repo
  frontend_test
  master
* frontend_details # current branch

$ git branch new_branch  # create new branch

# !!! Please commit/restore before checkout
$ git checkout new_branch # go to branch


$ git branch
  frontend_test
* new branch
  master
  frontend_details
```
```sh
# go back to look at commit
$ git checkout a6f95 # can checkout commit with first ~4 chars
$ git status
HEAD detached at a6f95ec # head is at a6f95ec
nothing to commit, working tree clean

$ git checkout master # go back
```

## Git Merge
Merge all changes from one branch to another
```sh
$ git branch
  new_feature
* master
$ git merge new_feature    # put new_feature's edits into master 
```
```
   .->[5]->[6]---.     <--- new_feature
   |             v
[1]-->[2]->[3]->[4]    <--master
```
### [Note]: Merge conflicts
```sh
$ git checkout master
$ git merge new_branch
Auto-merging merge.txt
CONFLICT (content): Merge conflict in merge.txt
Automatic merge failed; fix conflicts and then commit the result.
# Auto merge failed

$ git status
On branch master
You have unmerged paths. # merge conflict
(fix conflicts and run "git commit")
(use "git merge --abort" to abort the merge)

Unmerged paths:
(use "git add <file>..." to mark resolution)

both modified:   merge.py
```
```py
#--- BEGIN merge.py ---#
print("In both")              # Unrelated
<<<<<<<< HEAD  
print("Hello there")          # In current branch
=======
print("General Kenobi!")
print("You are a bold one")   # In new branch
>>>>>> new_branch
#--- END merge.py ---#
```
To solve: 
1. remove ===,<<<,>>>
2. make changes
3. commit
```
git commit -m "Fixed merge conflict
```


****
## Git Remote
Modify remote repositories
```sh
$ git remote -v # show remote
origin      git@github.com:abc/site.git (fetch) # "origin" repo
origin      git@github.com:abc/site.git (push)
production   server:/srv/git/mysite.git (fetch) # "server" repo
production   server:/srv/git/mserverysite.git (push)

# add new remote repo
$ git remote add production aws.com:/srv/0da83/auction.git
$ git remote rm production # remove remote repo
```

## Git Pull
Pull from server
```sh
# !!! Please commit changes before pull
# !!! May cause merge conflicts
$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://server.com/git/example.git
* branch          master         -> FETCH_HEAD
  56ed344..af830d7
Fast-forward
 test.html | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```

## Git Push
Push changes to remote repository
```sh
# !!! Please pull before push
$ git push # push, default
$ git push origin # push to remote repo "origin"
$ git push origin/master # push to remote repo "origin" branch "master"
```
### [Note]: Push errors
Error: No remote repository
```sh
$ git push
fatal: No configured push destination.
Either specify the URL from the command-line or configure a remote repository using

    git remote add <name> <url>

and then push using the remote name

    git push <name>
$ git remote add remote_dir user@server:/path
```
Error: No remote branch for current branch
```sh
$ git push
fatal: The current branch <branch_name> has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin <branch_name>
$ git push --set-upstream origin <branch_name> # do only once
...
$ git push
```