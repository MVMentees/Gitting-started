# Gitting-started
Intro Repo for Mentees containing information on getting started with git

[Video on creating VM](https://vimeo.com/1231025259/94fdee604d?fl=ip&fe=ec&share=copy)


# Git Cheat Sheet

## Daily Workflow
```bash
git pull
git status
git add .
git commit -m "Description of change"
git push
```

## Status
```bash
git status
```

## Pull Latest Changes
```bash
git pull
git pull origin main
```

## Add Files
```bash
git add file.txt
git add .
git add -A
```

## Commit Changes
```bash
git commit -m "Description of change"
```

## Push Changes
```bash
git push
git push origin main
```

## Branches

### Show Branches
```bash
git branch
git branch -a
```

### Create Branch
```bash
git checkout -b my-feature
# or
git switch -c my-feature
```

### Change Branch
```bash
git checkout main
# or
git switch main
```

### Delete Branch
```bash
git branch -d my-feature
git branch -D my-feature
```

## Restore / Undo

### Restore File
```bash
git restore file.txt
```

### Unstage File
```bash
git restore --staged file.txt
```

### Restore Everything
```bash
git restore .
```

### Undo Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```

### Undo Last Commit (Discard Changes
