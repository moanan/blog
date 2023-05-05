---
title: Repair git repo history
date: 2021-03-21 12:37:58
tags: 技术
---

Sometimes you mess up with your git repo history, and want a quick fix.

<!--more-->

## Untrack files already added to git repository
[Original post](http://www.codeblocq.com/2016/01/Untrack-files-already-added-to-git-repository-based-on-gitignore/)

### Step 1: Commit all your changes
Before proceeding, make sure all your changes are committed, including your .gitignore file.

### Step 2: Remove everything from the repository
To clear your repo, use:

```bash
git rm -r --cached .
```
- `rm` is the remove command
- `-r` will allow recursive removal
- `–cached` will only remove files from the index. Your files will still be there.
- The `.` indicates that all files will be untracked. You can untrack a specific file with git `rm --cached foo.txt`.
The `rm` command can be unforgiving. If you wish to try what it does beforehand, add the `-n` or `--dry-run` flag to test things out.

### Step 3: Re-add everything
```bash
git add .
```

### Step 4: Commit
```bash
git commit -m ".gitignore fix"
```

Your repository is clean :)

## Clean GIT history with BFG Repo-Cleaner
[Original post](https://medium.com/@vs28031996/remove-git-history-with-bfg-repo-cleaner-866808826eea)

BFG is faster and simpler way for Removing Big Files, Passwords, Credentials & other private data. It will rewrite the complete commit history i.e. all the commit hash, branch/tags refs will get changed without interfering with commit message and branch names.


### Step 1: Clean/backup latest commit and push
The BFG tool does not touch the latest commit, so you must first remove all the secrets from the repo, such that the latest commit is a clean commit (free of any secrets.)
Backup your GitHub repository first
Mirror Clone your repo as backup if things go ugly and rename it as your-git-repo-backup.git
Clone Repository
Mirror-clone/bare-clone (Learn the difference if you don’t know) your repository.
```bash
git clone --mirror path-to-your-git-repo.git
```

### Step 2: Download BFG
BFG: https://rtyley.github.io/bfg-repo-cleaner/
Download the JAR and put it in the same folder where you are going to clone your bare/mirror repo.
You need to have at least Java 7 installed on your computer in order for this to work.

### Step 3: Usage
#### To Strip Large blobs
To delete all files which are more than 10Mb in size
```bash
java -jar bfg.jar --strip-blobs-bigger-than 10M your-git-repo.git
```
#### To Remove file from history
```bash
java -jar bfg.jar -- delete-files file-to-be-deleted.php your-git-repo.git
java -jar bfg.jar -- delete-files {*.config,*.xml} your-git-repo.git
```
#### To remove Folder from history
```bash
java -jar bfg.jar - — delete-folders {OldConfigsFolder} your-git-repo.git
```

### Step 4: Overwrite history
Force git to garbage-collect inside the mirror repo
```bash
cd your-git-repo.git
git reflog expire - expire=now - all && git gc - prune=now - aggressive
```
In the end, you need to run
```bash
git push -f
```