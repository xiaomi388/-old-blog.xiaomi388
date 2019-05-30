---
layout: post
title: Review of Git
tags: [Git]
---

The post represents notes I took when I am learning git. Sorted by timeline.

### 2019-05-30

#### git log filter

 Filters include `--author`, `--grep`, `--author`...

`--all-match` means limits the output to just those commits that match all .

`-S` Only show commits adding or removing code matching the string.

`--no-merges` prevents the display of merge commits.

#### Undoing things

```
git commit -amend # takes your staging area and uses it for the commit.
```

As a result, the second commit replaces the results of the first.

#### Unstaging a Staged File

```
git reset HEAD xxx.py
```

This command is used to unstage a specific file.

**This is a dangerous command**

#### Unmodifying a Modified FIle

```
git checkout -- <file>
```

**This is a dangerous command**

Don't ever use this command unless you absolutely know that you don't want those unsaved local changes.

#### Showing Your Remotes

```
git remote # to see the server name
git remote -v # shows shortnames for reading and writing
```

The default short name Git gives to the server we cloned from is *origin*

#### Add Remote Repositiories

```
git remote add <shortname> <URL>
```

#### Alter Remote Repositiories

```
git remote set-url <shorname> <URL>
```

#### Fetching and Pulling from Remotes

```
git fetch <remote>
```

fetch all the data from remote project.