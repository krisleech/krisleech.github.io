---
layout: post
title: "Remote secure storage"
date: 2013-02-14 21:01
comments: true
published: true
categories: [ruby]
---

Two examples using TrueCrypt and an SSH file system of remotely storing files
securely.

<!--more-->

## TrueCrypt

TrueCrypt is a cross platform encryption software which allows you to mount an encrypted volume. It works in much the same way as a Mac encrypted voume which had been working well except it didn't feel right using a format which wasn't easily used on Linux as well. I had been thinking about moving away from Apple since they seem to be moving more and more towards restricting the software I can run on my Mac.

Anyway, TrueCrypt is excellent, it creates a single file which you can double click and after entering the password it mounts a volume which works like any other folder on your Mac.

The only problem is I needed to share the volume across multiple (well two) Mac's. My first though was to stick the file in Dropbox. This worked well for a while until I noticed duplicates of the file suffixed "conflicted". The problem was the volume, which is actually a single file was not sync'd until the volume was unmounted. So if I had the volume open on two PC's and changed anything in both I would get a conflict after both where unmounted.

## SSH File System

Now I've started playing with SSHFS, which is a OSXFuse extension which allows you to mount SSH as a volume.

Create new user on your server and add your SSH key
Create a new folder somewhere in your home directory, e.g ~/secure_storage
Create a script for SSHFS which mounts the volume, `chmod +x` the script

```
#!/bin/zsh
 
sshfs secure_drive@example.com: ~/secure_drive
open ~/secure_drive
```

It you want a cool icon for the script, try here.
