---
layout: post
title: "Upgrading an unsupported Ubuntu"
date: 2013-06-19 21:01
comments: true
published: true
categories: [ruby]
---

Upgrading Natty (11.04) the same should apply to other upgrades.

<!-- more -->

I use Rackspace which replaces the default apt repositories with its own
mirrors. Once a Ubuntu becomes unsupported Rackspace removes the repository and
installing any software results in 404 errors.

First backup the entire server as an image incase something goes wrong.

Next create a copy of the sources file for apt-get:

```bash
$ cp /etc/apt/sources.list ~/sources.list.backup
```

Edit source.list:

```bash
$ vi /etc/apt/sources.list
```

Replace all occurances of "mirrors.rackspace.com" with "old-releases.ubuntu.com" in "/etc/apt/sources.list", then:

```bash
$ sudo apt-get update
```

And follow the usual upgrade path:

```bash
$ sudo apt-get install update-manager-core

$ sudo do-release-upgrade
```

I'd recommend running `do-release-upgrade` in a tmux session so it is not interrupted if you net connection dies.
