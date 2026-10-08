# Gitting-started
Intro Repo for Mentees containing information on getting started with git

[Video on creating VM](https://vimeo.com/1231025259/94fdee604d?fl=ip&fe=ec&share=copy)

# Git Cheat Sheet


## Check changes made and commit or restore them

|Command|Description|
|---|---|
|git status|Shows branch you are on and status of changes|
|git diff --name-only|Show the names of all items changed|
|git diff|Shows all changes made in detail|
|git diff /path/to/item|Shows changes to that specific item|
|git add -A|Add all changes to staged files|
|git restore --staged \<filename\>|remove \<filename\> from staged files|
|git restore \<filename\>|Put the \<filename\> item back to the way it was during last commit|
|git commit -a|Commit all staged items and take you to a comment and accept item that<br>when saved actually commits.|

## Branches

|Command|Description|
|---|---|
|git branch -a|Show all the branches here and remote|
|git switch <existing_branch>|switch to an existing branch like main|
|git switch -c <new_branch>|Create then switch to the new_branch|
|git merge <branch_name>|Merge the branch_name into the branch I am currently on|
|git branch -d <branchname>| delete the branch_name|

### Typical merge sequence
```text
git switch main
git pull
git merge <branchname>
git push
git branch -d <branchname>
```

## Logs and history
|Command|Description|
|---|---|
|git log|Show log for the current branch|
|git log --oneline|Give first line of commit message only|
|git log --stat|Show log along with items impacted by commit|
