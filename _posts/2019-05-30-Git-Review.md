---
layout: post
title: Review Of Git
tags: [Git]
---

The post represents notes I took when I was learning git. Sorted by timeline.

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

#### Adding Remote Repositiories

```
git remote add <shortname> <URL>
```

#### Altering Remote Repositiories

```
git remote set-url <shorname> <URL>
```

#### Renaming and Removing Remotes

```
git remote rename <shortname> <shortname>
git remote remove <shortname>
```

#### Fetching and Pulling from Remotes

```
git fetch <remote>
```

fetch all the data from remote project.

**Note: git fetch doesn't automatically merge it with any of your work or modify what you're currently working on**

```
git pull <shortname> <branch>
```

pull means fetch and then merge that remote branch into your current branch.

```
git pull
```

git pull will automatically **merge in the master** branch on the remote after it fetches all the remote references, and then merge that remote current branch into your current branch. git push is the same. e.g:

```
android_puzzle_server git:(framework) git remote show origin
* remote origin
  Fetch URL: https://github.com/xiaomi388/android-puzzle-se.git
  Push  URL: https://github.com/xiaomi388/android-puzzle-se.git
  HEAD branch: master
  Remote branches:
    framework                        tracked
    handler                          tracked
    master                           tracked
    refs/remotes/origin/ding         stale (use 'git remote prune' to remove)
    refs/remotes/origin/user-handler stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    framework merges with remote framework
    master    merges with remote master
  Local refs configured for 'git push':
    framework pushes to framework (up to date)
    handler   pushes to handler   (local out of date)
    master    pushes to master    (local out of date)
```

#### Push

If you and someone else clone at the same tiem and they push upstream and then you push upstream, your push will rightly be rejected. **You'll have to fetch their work first and incorporate it into yours before you'll be allowed to push**

#### Inspecting a remote

```
git remote show <shortname>
```

#### Tagging

usage: marking release points.

##### Listing Tags

```
git tag [-l "foo"]
```

##### Annotated Tags

Annoteated Tags are objects, **stored as full objects**. They contain dates, taggers and so on.

```
git tag -a v1.4 0m "my version 1.4"
```

##### Lightweight Tags

Lightweight tags do not supply any of the -a, -s, -m ops, just provide a tag name.

```
git tag v1.4
```

##### Showing a specific tag

```
git show v1.4
```

