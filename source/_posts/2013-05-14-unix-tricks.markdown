---
layout: post
title: "Unix / Shell tricks"
date: 2012-02-19 21:01
comments: true
published: true
categories: [shell, unix]
---

Find your public IP in the shell
curl -s http://checkip.dyndns.org | sed 's/[a-zA-Z/<> :]//g'

Simple but effective.

Chaining commands
Using `;` will run the next command even if the first one fails (exit code is -1)

```
cd some_folder; tar -c .
```

Using `&&` will only run the next command if the first one is okay (exit code not -1)

```
cd some_folder && tar -c .
```

Pipes are used to connect the STDOUT of one fork to the STDIN of another

```
(cd some_folder && tar -c .) | (cd another_folder && tar -x)
```

^ Here we have two forked child processes, the second one will wait until it receives input from the Pipe.
