---
layout: post
title: "Git stuff"
date: 2013-05-14 21:01
comments: true
published: true
categories: [git]
---

Remove untracked files

```
git clean -f -d
```

This will remove all untracked files and directories, great it you run a generator without thinking!

Displaying history and status

```
git log --oneline --decorate
git status -sb
```

Delete all merged branches

To how all merged branches:

```
git branch --merged
```

To delete all merged branches:

```
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

Branches containing commit

```
git branch --contains <commit>
```

Quickly rebase a feature branch

While on a feature branch instead of switching to the next branch, pulling, swithcing back to the feature branch and doing a rebase just do:

```
git pull --rebase origin next
```

Checkout a remote branch

```
git checkout -b second/next second/next
```

This checkout the next branch on the remote named second in to a local branch named second/next, which will not comflict with an existing local branch named next.
