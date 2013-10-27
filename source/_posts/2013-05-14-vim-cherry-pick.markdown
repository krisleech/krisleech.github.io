---
layout: post
title: "Cherry Picking"
date: 2012-03-07 21:01
comments: true
published: true
categories: [vim]
---

I use cherry pick in a number of situations, for example imagine I an in a feature branch and I encounter a minor bug unrelated to the feature I'm working on. I fix the bug and commit, but only then realise I should have comitted it to master instead so everyone gets the fix.

<!-- more -->

In this situation I would stash or commit my work in progress on the current branch. Checkout master and cherry pick the single commit which fixed the bug. I can then go back to my branch, revet the bug fix commit and rebase from master (however normally I would just leave the bug fix and get on with it!).

```bash
git checkout master
git cherry-pick h5657h8
git push origin master
```

Merging in part of a pull request
Sometimes Github pull requests contain more commits than you want to merge, instead of letting Github handle the pull request, do it yourself:

```bash
git remote add humanshell https://github.com/humanshell/vimfiles.git
git fetch humanshell
git cherry-pick e27369b
git remote rm humanshell
git push origin master
```
