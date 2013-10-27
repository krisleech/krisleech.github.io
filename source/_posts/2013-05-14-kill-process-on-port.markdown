---
layout: post
title: "Kill a process by port"
date: 2013-05-14 21:01
comments: true
published: true
categories: [ruby]
---

Kill a process by port

```
fuser -k 9000/tcp
```

This replaces manually grepping for process id and killing it:

```
ps aux | grep unicorn
kill -9 PID
```

Note: This does not work on MacOS.

Example usage:

```
#!/bin/sh
echo "Restarting Unicorn on port 9000"
fuser -k 9000/tcp
while nc -vz localhost 9000; do sleep 1; done
bundle exec unicorn_rails -p 9000 -E production -D
while ! nc -vz localhost 9000; do sleep 1; done
curl http://localhost:9000
```
