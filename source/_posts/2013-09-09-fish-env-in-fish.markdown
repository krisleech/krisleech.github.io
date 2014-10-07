---
layout: post
title: "Setting ENV in Fish shell"
date: 2013-09-09 21:01
comments: true
published: true
categories: [fish shell]
---

Setting `JAVA_HOME` in fish:

```
set -U JAVA_HOME (which java)
```

* `-U` is universal scope, persisting beyond the current process to sub-processes.
* `(...)` is an eval.

Ref: http://fishshell.com/docs/current/index.html#variables
