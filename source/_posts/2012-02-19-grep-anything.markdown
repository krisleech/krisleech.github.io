---
layout: post
title: "Grep anything"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Grep anything
19/02/2012

Grep in file (with context)

```
fgrep jerry@example.com production.log -B 5 -A 5
```

Grep in Files

```
grep -r word .
```

search for word in all sub-directories (recursive)

Grep in tail

```
tail -f log/development.log | grep delete
```
