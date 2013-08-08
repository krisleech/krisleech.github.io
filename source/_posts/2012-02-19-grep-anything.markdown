---
layout: post
title: "Grep anything"
date: 2012-02-19 21:01
comments: true
published: true
categories: [ruby]
---

## Grep in file (with context)

5 lines before mtach, 5 lines after

```
fgrep jerry@example.com production.log -B 5 -A 5
```

## Grep in Files

search for word in all sub-directories (recursive)

```
grep -r word .
```

## Grep in tail

```
tail -f log/development.log | grep delete
```

## Grep processes

```
pgrep tmux
```
