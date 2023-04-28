# Just Git Things: A Git Guide By Me

## What is Git? and Git vs. GitHub
Sorry, this guide is not an intro to git. It's a guide.
- https://www.geeksforgeeks.org/difference-between-git-and-github/
- https://devmountain.com/blog/git-vs-github-whats-the-difference/

## Git Commands
### config
_Configure options for repository or globally_.
- https://git-scm.com/docs/git-config
```
git config --global user.name "My Name"
```
Then check to make sure the name was set correctly.
```
$ git config --global user.name
> My Name
```
You may also want to set your `user.email`.
#### aliases
_Configure git aliases_. Examples:
```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.st status
```
For example, instead of typing `git status` you would type `git st`.
- https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases
### clone
_Clone a repository into a local directory_.
- https://git-scm.com/docs/git-clone
```
git clone [repository]
```
### status
_See the status of the current local directory compared to remote directory_.
```
git status
```
- https://git-scm.com/docs/git-status
### branch
_See the branches you currently have stored locally and highlight which branch you’re on_.
```
git branch
```
- https://git-scm.com/docs/git-branch

#### Delete a branch local or remote

Local:
```
git branch -d [local branch]
```
or if you want to force delete it (this will delete any changes you've made to files on that branch):
```
git branch -D [local branch]
```

Remote:
```
git push origin --delete [remote branch]
```
### checkout, switch, restore
- https://git-scm.com/docs/git-checkout
- https://git-scm.com/docs/git-switch
- https://git-scm.com/docs/git-restore

Some links about the overlapping functions of `git checkout`, `git switch`, and `git restore`
- https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch
- https://stackoverflow.com/a/58003889

#### Switch to a branch
Checkout: `git checkout [branch]`\
Switch: `git switch [branch]`

#### Create a new branch.
Checkout: `git checkout -b [new branch name]`\
Switch: `git switch -c [new branch name]` or `git switch --create [new branch name]`

#### Access state of code at commit
`git checkout [commit hash]`\
This will detach HEAD and update index and files in working tree, so the working tree will contain your local changes and the state recorded in the commit.

#### Restore working tree file to what it is at HEAD
Restore: `git restore [file]`\
Checkout: `git checkout -- [file]`. The `--` indicates that it's a file.

#### Restore a staged file to working tree
`git restore --staged [file]`

#### Restore a file from a branch (changes that file to be the version on the specified branch)
`git restore –source [branch] [filename]`\
- Example: `git restore –source origin/main package.json`
### add
_Add files to be staged for commit_.
- https://git-scm.com/docs/git-add

Add entire directory recursively, including untracked files but not including files in `.gitignore`.
```
git add .
```
Add all files in directory (except hidden files that start with `.`). _(`*` is shell functionality)_
```
git add *
```
Add certain changes made in files. like `git add` but allows you to go through each (c)hunk _(what is a hunk??)_ and choose what to stage and what not to stage.
```
git add -p [filename or `.`]
- OR -
git add –-patch [filename or `.`]
```
### rm
- https://git-scm.com/docs/git-rm

_Remove files from working tree and index_.
```
git rm [file]
```

_Remove files from tracked to untracked_.
```
git rm --cached [file]
```
### commit
_Commit files that are staged_.
```
git commit
```
- https://git-scm.com/docs/git-commit

_Commit with commit message_. If you don't do this, a text editor will open up (vim) where you will have to type a message.
```
git commit -m “[insert message in quotes]”
```
_Add all files that have been modified or deleted and then commit with message indicated_.
```
git commit -am “[insert message in quotes]”
```
_Amend your latest commit with these changes_. It’ll ask you to update your commit message. useful if you realize you made a small mistake or if you want to add something that is relevant to your latest commit.
```
git commit --amend
```
### push
_Update remote with changes that were committed to local branch_.
```
git push
```
- https://git-scm.com/docs/git-push

If you're pushing from a newly created local branch,
`git push --set-upstream origin [intended branch name]` or
`git push -u origin [intended branch name]`

#### Force push
used when you've rewritten git history
```
git push --force-with-lease
```
According to the docs,
> `--force-with-lease` will protect all remote refs that are going to be updated by requiring their current value to be the same as the remote-tracking branch we have for them. If somebody else built on top of your original history while you are rebasing, the tip of the branch at the remote may advance with their commit, and blindly pushing with `--force` will lose their work.

It's basically a safer way to force push than using just `--force`.
### pull
_Pull from remote directory to update local directory_ (under the hood, it's a [`fetch`](#fetch) and a [`merge`](#merge)/[`rebase`](#rebase). Note: there may be conflicts you have to resolve.
```
git pull
```
- https://git-scm.com/docs/git-pull
### fetch
_Fetch and download from remote repo to local repo_.
```
git fetch
```
- https://git-scm.com/docs/git-fetch
#### locally grab a remote branch from same repo
```
git fetch origin [branch]
git checkout --track origin/[branch]
```
`--track` sets "upstream" tracking configuration for the new branch (from [git docs](https://git-scm.com/docs/git-branch#Documentation/git-branch.txt---trackdirectinherit))\
`track origin` indicates the remote branch to track, in this case it’s the origin of the branch
### merge
_Merge changes with other changes_.
```
git merge [source branch]
```
- https://git-scm.com/docs/git-merge

`merge commit` - keep source commits of working branch\
`merge squash` - remove all commits of child branch and squash into a single commit when merging into another branch\
`merge conflicts` - conflicts arise between the two versions, and you have to resolve them
### rebase
_Reapply commits on top of another base tip_.
```
git rebase
```
tbh hard to describe, but basically does what merging does but slightly differently bc it rewrites git history (and does some other things too)
- https://git-scm.com/docs/git-rebase

```
git rebase [branch to get changes from]
- OR -
git rebase [branch to get changes from] [branch to put changes in]
```
Example:
```
git rebase master
git rebase master topic
```
- NOTE: `git rebase master topic` is a short-hand of `git checkout topic` followed by `git rebase master`. When `rebase` exits, topic will remain the checked-out branch. (from git-scm.com)

For more information about rewriting history, see [rewriting history](#rewriting-history).

TODO: a cool diagram drawn by some rly cool ppl

#### rewriting history
Pick and choose what is rebased using `--interactive` or `-i` flag. Rebase `n` most recent commits.

```
git rebase -i HEAD~n
```

Here is an example of what would pop up after running `git rebase -i HEAD~2`:
```
pick 59013de Add a feature
pick ee306e9 Edit a typo in that feature
```
##### squash
_Squash commits together to rewrite git history_.
To squash the second commit into the first:
```diff
pick 59017de Add a feature
- pick ee386e9 Edit a typo in that feature
+ squash ee386e9 Edit a typo in that feature
```
- Helpful link for squashing: https://levelup.gitconnected.com/how-to-squash-git-commits-9a095c1bc1fc

##### drop
_Drop commits that you don’t want to keep_.
To drop the second commit:
```diff
pick 59017de Add a feature
- pick ee386e9 Edit a typo in that feature
+ drop ee386e9 Edit a typo in that feature
```
- Why drop instead of deleting the whole line? https://stackoverflow.com/questions/35846154/git-rebase-interactive-drop-vs-deleting-the-commit-line

TODO: A slightly less cool diagram diagram about interactive rebasing with multiple branches branched off of each other drawn by a slightly less cool person:

##### other commands
Commands (from https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History):
```
p, pick <commit> = use commit
r, reword <commit> = use commit, but edit the commit message
e, edit <commit> = use commit, but stop for amending
s, squash <commit> = use commit, but meld into previous commit
f, fixup <commit> = like "squash", but discard this commit's log message
x, exec <command> = run command (the rest of the line) using shell
b, break = stop here (continue rebase later with 'git rebase --continue')
d, drop <commit> = remove commit
l, label <label> = label current HEAD with a name
t, reset <label> = reset HEAD to a label
m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
.       create a merge commit using the original merge commit's
.       message (or the oneline, if no original merge commit was
.       specified). Use -c <commit> to reword the commit message.
```
### revert
_Revert to past commit; creates a new commit to make changes, does not change commit history_.
```
git revert [commit hash]
```
### reset
_Delete commits up to specified commit_.
```
git reset [commit hash]
```
-  https://git-scm.com/docs/git-reset

`git reset --hard` - deletes commits and changes; any changes are discarded; cannot be undone\
`git reset --mixed` - reset the index but not the working tree\
`git reset --soft` - reset local HEAD to point to commit; leaves all your changed files "Changes to be committed"
- Example: `git reset --soft HEAD~3` rewinds 3 most recent commits and restore to staged
### diff
_Diff your local changes with the commits_.
```
git diff
```
- https://git-scm.com/docs/git-diff

Diff the local specified file/directory with the commits file/directory.
```
git diff -- [file or directory path]
```
Show changes for files currently in staging area vs. remote HEAD.
```
git diff –-cached
```
Show changes made to a renamed file compared to the old file name HEAD refers to.
```
git diff HEAD:[old_file_name] [new file name]
```
- https://stackoverflow.com/questions/5730460/how-to-do-a-git-diff-on-moved-renamed-file
### log
_Show log of commits_.
```
git log
```
- https://git-scm.com/docs/git-log

`git log --oneline` shows all commits in one line each
`git log --pretty=oneline` shows all commits in one line but pretty
### show
Show changes in a commit.
```
git show [commit hash]
```
- https://git-scm.com/docs/git-show
### stash
- https://git-scm.com/docs/git-stash
_Stash local changes in working directory_.
```
git stash
```
Stash with a message.
```
git stash -m “[message]”
```
Pop the most recent stash (stash is a stack).
```
git stash pop
```
List all stash entries
```
git stash list
```
_Show files changed_ in the most recent stash entry as a diff between the stashed contents and the commit back when the stash entry was first created.
```
git stash show
```
_Show all changes_ in most recent stash entry.
```
git stash show -p
```
_Show all changes in stash `n`_.
```
git stash show -p stash@{n}
```
- https://howto.lintel.in/how-to-see-stashed-changes-using-git-stash/
https://stackoverflow.com/questions/7677736/git-diff-against-a-stash

_Apply a stash without popping it_.
```
git stash apply
```
_Reverse an apply_.
```
git stash show -p | git apply --reverse
```
- How to reverse an apply https://stackoverflow.com/a/1021867

_Drop/delete the most recent stash entry_.
```
git stash drop
```

_Drop/delete stash entry `n`_.
```
git stash drop stash@{n}
```
### cherry-picking
_For when you mess up a branch and need to pick and choose commits to keep changes from_.
- https://git-scm.com/docs/git-cherry-pick
- Great tutorial!!: https://www.atlassian.com/git/tutorials/cherry-pick
```
git cherry-pick [commit hash]
```
You can find the git hash by running `git log`. You don't need the full hash you can take the first 8(?).

You can also cherry pick _multiple commits_: https://stackoverflow.com/questions/1670970/how-to-cherry-pick-multiple-commits
To cherry-pick all the commits from commit A to commit B (where A is older than B)
`git cherry-pick A^..B` includes commit A
`git cherry-pick A..B` excludes commit A
- A should be older than B, or A should be from another branch, or else it will fail
- On Windows, it should be A^^..B as the caret needs to be escaped, or it should be "A^..B" (double quotes).
- In zsh shell, it should be 'A^..B' (single quotes) as the caret is a special character.

Cherry pick the changes in the commit(s) but move the contents into the working tree and the index. Super useful!
```
git cherry-pick --no-commit [commit hash]
```
or use `-n`

Edit commit message before commiting
```
git cherry-pick -edit [commit hash]
```
or `-e`

### tagging
- https://git-scm.com/book/en/v2/Git-Basics-Tagging

_Tag certain versions or states of repo/create pointers to commits that never move_.

Show list of existing tags.
```
git tag
```
_Lightweight tag_. This is basically the commit checksum stored in a file.
```
git tag [tag title]
```
_Annotated tag_.
```
git tag -a [tag title] -m “[message]”
```
### cleaning a branch
_Rewrite branches_. not just history, but for cleaning a branch.\
big scary, don’t do it unless you’re doing it on a practice branch/know what you’re doing
```
git filter-branch
```
- https://git-scm.com/docs/git-filter-branch
- https://rtyley.github.io/bfg-repo-cleaner

Example use: if someone accidentally pushes a secret to a remote (public) repo
### blame
_Show who made recent changes to file_.
```
git blame [file]
```
### gitignore
`.gitignore` specifies files to ignore (files to not track)
- https://git-scm.com/docs/gitignore

### Submodules
- https://git-scm.com/book/en/v2/Git-Tools-Submodules

#### Add a submodule to an existing repo
```
git add submodule [HTTPS or SSH url of the repo]
```
#### Get submodules of a repo you've already cloned
```
git submodule init
git submodule update
- OR -
git submodule update --init
```
#### Clone a project with submodules
```
git clone --recurse-submodules [HTTPS or SSH url of the repo]
```
If the submodules have submodules (nested submodules):
```
git submodule update --init --recursive
```
#### Pull in changes from the submodule remote repo.
```
git submodule update --remote
```
#### Pull in changes from the main project remote repo
```
git pull
git submodule update --init --recursive
```
All submodules can be found in `.gitmodules`.
#### Remove Submodule
- https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule
```
git rm [path to submodule]
```
After doing this, the submodule will still be in `.git/modules` directory and in the git config `.git/config`. To remove it from those:
```
rm -rf .git/modules/[path to submodule]
git config --remove-section submodule.[path to submodule]
```
### Track an existing remote branch to an existing local branch
while on local branch of same name: `git branch -u origin/[remote branch name]`
if commits are ready to push: `git push -u origin [remote branch name]`

### Update local branch with changes made to `main`:
Let’s say you are working on a feature in a branch called `my_branch`. You look in Github and notice that a pull request (also known as a merge request) has been merged into the main branch. This means that there is code in main that isn’t in my_branch. In other words, `my_branch` is “behind” the main branch.

- Commit any local changes
- `git switch my_branch`
- `git pull origin main`
- `git checkout my_branch`
- `git merge main`

If you have any merge conflicts, you can resolve them in VSCode or via the command line. Then `add` and `commit`.
## Git Resources
- [Git documentation](https://git-scm.com/doc)
- [Great guide that explains each command well](https://rogerdudler.github.io/git-guide/)
- [Oh shit, Git](https://ohshitgit.com/) - for when you mess something up and need to fix it
- [Git Handbook](https://guides.github.com/introduction/git-handbook/)
- [Videos and explanations](https://www.simplilearn.com/tutorials/git-tutorial/git-commands) explaining basic git commands, difference between Git and GitHub, and some other stuff

Videos:
- [What is Git?](https://youtu.be/2ReR1YJrNOM) (2 min video)
- [Git for beginners](https://www.youtube.com/watch?v=DVRQoVRzMIY) (43 min video)
- [Git/GitHub for beginners](https://youtu.be/3fUbBnN_H2c) (LONG video)
- [Git for beginners](https://youtu.be/8JJ101D3knE) (1 hour video)

## References that gave me info for this (in addition to links scattered throughout)
- My own knowledge
- Git docs
- My friends
- Offensive Security course at Tufts (https://www.cs.tufts.edu/t/courses/description/spring2022/CS/151-01)
- Fundamentals of Git - MITRE MTN Thursday Training Series
- [Alex Z.](https://github.com/alexz12948)
- Sarah O.
- Julia A.
- Matt G.
