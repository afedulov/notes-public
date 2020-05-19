GIT
========================


```bash
git checkout -b branch origin/branch	#	checkout remote branch and track as local

git clean -df					removes all untracked files   
git checkout -- .				clears all unstaged changes  

git diff --cached > name.diff			# diff staged, but uncommited files (including newly created)
git apply name.diff				# apply diff



$ git checkout -b feature_branch_name		# git create local branch
$ git push -u origin feature_branch_name	# push local branch to remote and track


git rebase master 				# being on a feature branch, rebase feature changes
git rebase origin/master 			# on top of master / origin/master

git checkout --ours PATH/FILE
git checkout --theirs PATH/FILE

gitg --all

```

### Git revert non-pushed merge (to rebase instead of merge)
```
git reset --hard HEAD~1
```

### Keep feature branch up to date with master

```bash
git checkout master  
git pull  
git checkout FEATURE_BRANCH  
git rebase master 				# and resolve conflicts  
git rebase --continue  
git push -f 					# force branch override, because remote branch sees different common ancestors and do not know what to do with it
```


### Move last commit from one branch to another (A (c)-> B)

```bash
git checkout B
git cherry-pick c-hash
git checkout A
git rever c-hash
```

### GIT Bash Prompt and Editor setup

```
source ~/.git-prompt.sh
export PS1="\[\033[00m\]\u@\h\[\033[01;33m\] \w \[\033[31m\]\$(__git_ps1)\[\033[00m\]$\[\033[00m\] "
export VISUAL=vim
export EDITOR="$VISUAL"
```

[ .git-prompt.sh](media/.git-prompt.sh)

### Revert commits:

```bash
git revert --no-commit 08410a6a488c..HEAD     # revert everything from HEAD to 08410a6a488c non-inclusive 08410a6a488c
```


### Revert changes to modified files.
```bash
git reset --hard
```

### Remove all untracked files and directories. (`-f` is `force`, `-d` is `remove directories`)

```bash
git clean -fd
```

###Copy all files tracked by git
```bash
git archive master | tar -x -C /path/to/output
```
### Git Rebase
```bash
git checkout feature
git rebase -i master
git checkout master
git merge --squash feature # Add just files (without commit)
git merge feature #Add feature commits (when squashed feature commits before)
```

### Revert changes to modified files.
```bash
git pull --rebase
```

### Git Rebase faster alternative
```bash
git checkout feature
git fetch origin # fetches all
git rebase origin/master
```

### Git Aliases:
```bash
git config --global alias.pr 'pull --rebase'
```

### Git revert changes to specific file to previous version
```bash
git checkout HEAD~1 automation/docker/docker-compose.yml
```


### Amend previous commit with no commit message edits
```bash
git commit --amend --no-edit
```

### Checkout pull request locally
```bash
git fetch origin(or upstream) pull/2/head:pr-2
git checkout pr-2
git pull origin(or upstream) pull/2/head # to update
```
### Setup upstream repo for a fork
```bash
git remote -v
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
or
git remote add upstream git@github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

### Sync branch with upstream master
```bash
 git checkout master
 git fetch upstream
 git merge upstream/master
 git push
 git checkout BRANCH_NAME
 git rebase master
```

### Resolve rebase conflicts and push
```bash
 git checkout master
 git fetch upstream
 git merge upstream/master
 git push
 git checkout BRANCH_NAME
 git rebase master # and resolve conflicts
 git push -f origin BRANCH_NAME					# force branch override, because remote branch sees different common ancestors and do not know what to do with it
# NOTE When doing a push --force, always specify the branch name explicitly to avoid messing other branches in the repo  
```


### Disgard local diverged branch
```bash
$ git branch -D BRANCH_NAME
$ git checkout --track -b BRANCH_NAME origin/BRANCH_NAME
```

### Remove deleted remote branches
```bash
$ git remote prune origin
```

### Force english GIT
```
echo >> ~/.config/fish/config.fish  "\
set -x LC_ALL en_US.UTF-8
set -x LC_CTYPE en_US.UTF-8"
```

### Rebase your work on someone's fork
```
$ git remote add theirusername git@github.com:theirusername/reponame.git
$ git fetch theirusername
$ git checkout -b mynamefortheirbranch theirusername/theirbranch
```

### Undo commits pushed to public
```
git checkout -b branch-unmerge
git revert --no-commit branch-unmerge~3..branch-unmerge #3 - actual count of commits to be reverted
# To cancel: git revert --abort
# Result: A <- B <- C <- D <- BCD' <- HEAD
git commit -am "[hotfix] Remove erroneouysly merged commits"
git push --set-upstream origin branch-unmerge
# Open PR
```

### Fetch one remote branch
```
git fetch remotes/origin/flink-1.5.4-ms3 flink-1.5.4-ms3
```

### Idea as mergetool:
```
in ~/.gitconfig

[mergetool "idea"]
    cmd = /usr/local/bin/idea merge \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
    trustExitCode = false
[merge]
    tool = idea

$ git mergetool
```

### Idea as difftool:
```
[diff]
    tool = idea
[difftool "idea"]
    cmd = /usr/local/bin/idea diff $(cd $(dirname "$LOCAL") && pwd)/$(basename "$LOCAL") $(cd $(dirname "$REMOTE") && pwd)/$(basename "$REMOTE")


[difftool]
	prompt = false
```


### Import upstream branch into fork
```
git fetch upstream
git checkout -b newbranch upstream/newbranch
git push -u origin newbranch
```


### Git squash commits
https://dev.to/victoria/a-quick-guide-to-squashing-git-commits-7io
```bash
git rebase -i HEAD~5
```
Use squash to merge commit messages and fixup to skip.  
First commit has to be be always picked, but often the message from the last one is relevant.
Trick:  
 use reword on first commit (rename to last commit), squach the rest

### Abort rebase (or any other action)
```
git reflog
git reset --hard "HEAD@{8}"
```

### Aliases
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.tree 'log --graph --full-history --all --color --date=short --pretty=format:"%Cred%x09%h %Creset%ad%Cblue%d %Creset %s %C(bold)(%an)%Creset"'
```

### Remove last commits without loosing the changes
```
git reset HEAD~1 --soft   
```

### Modify older commit
1. Use `git stash` to store the changes you want to add. (optional)
2. Use `git rebase -i HEAD~10` (or however many commits back you want to see).
3. Mark the commit in question (`a0865...`) for edit by changing the word `pick` at the start of the line into `edit`. Don't delete the other lines as that would delete the commits.[^vimnote]
4. Save the rebase file, and git will drop back to the shell and wait for you to fix that commit.
5. Pop the stash by using `git stash pop`
6. Add your file with `git add <file>`.
7. Amend the commit with `git commit --amend --no-edit`.
8. Do a `git rebase --continue` which will rewrite the rest of your commits against the new one.
9. Repeat from step 2 onwards if you have marked more than one commit for edit.


### Stash one file and remove it from the working state (also stashes other files)
```
git add .
git reset thefiletostash
git stash save --keep-index
```

### See specific stash diff to HEAD
```
 git stash show -p stash@{0}
```

### Split an old commit into more than one changeset
```
git rebase -i <last good commit>
#Mark some changes as edit.
git reset HEAD^ # Move all changes from the currently edited commit into the working dir
git add <files you want to keep in this change>
git stash --keep-index
#Fix things up as necessary. Don't forget to git add any changes.
git commit
git stash pop
#Repeat, from #5, as necessary.
git rebase --continue
```

## Split an old commit into two based on patch hunks
```
git rebase -i <last good commit>
#Mark some changes as edit.
git reset HEAD^ # Move all changes from the currently edited commit into the working dir
# Press y for parts that you want to be in the next commit
git add -p (optional: <files you want to keep in this change>)
git commit (use --amend to reuse the previous commit message, or create a new message1)
git rebase --continue
```

### During an interactive rebase, find out where you are:
```
git show (cat .git/rebase-merge/stopped-sha)
cat .git/rebase-merge/message

cat .git/rebase-merge/git-rebase-todo # See remaining actions
cat .git/rebase-merge/onto # Hash of the target
```

### Find in which commit a file was deleted
```
git log --all --full-history -- "**/StackTraceSampleCoordinator.java"
git diff-tree --no-commit-id -r bd61ad98 # --name-only - only for names
git checkout bd61ad98~1 path/to/StackTraceSampleService.java # to restore
```

### Find in which commit a file was deleted/last modified and checkout
```
git rev-list -n 1 HEAD -- <file_path>
git checkout <deleting_commit>^ -- <file_path>
```

### Show file modification history
```
git lg --name-status -- "**/TaskStackTraceSampleableTaskAdapter.java"
```

## HUB
### Fork repo, move origin to fork:
```
hub fork --remote-name origin
git checkout -b feature
git push origin feature
hub pull-request
```

===========
### Current branch:
```set CURRENT_BRANCH (git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ \1/' | tr -d [:space:])```
