---
layout: post
title: "Remote Git Branches"
date: 2013-07-28 21:01
comments: true
published: true
categories: [git]
---

Some notes on managing remote branches

<!-- more -->

List remote branches

```
git branch -r
```

Checkout a remote branch

```
git fetch
git checkout -t origin/feature
```

Tracking links a local and remote branch so push/pull operations will be applied to both branches.
Without tracking the branch is just a independent copy of the remote branch and will not reflect future changes on origin.

-t is not nessesary if git config branch.autosetupmerge true

The master branch is automatically tracked.

If you forget to track a remote branch you can add tracking with:

```
git branch --set-upstream my_feature origin/next
```

Push a branch to remote

```
git push -u origin some_new_feature
```

This branch will automatically be tracked.

Pushing a branch to remote is not usually nessesary unless the code needs reviewing before being merged.

Delete a remote branch
Strange syntax alert!!

```
git push origin :newfeature
```

You will also need to delete the local branch.
