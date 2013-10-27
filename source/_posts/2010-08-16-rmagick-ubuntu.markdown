---
layout: post
title: "Rmagick"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

The latest version, 2, of RMagick requires ImageMagick version 6.4.9 or greater. Unfortunately this version is not available via Aptitude - at least not on the version I am running, which is a few years old now.

<!--more-->

The solution, for my purposes, was to install an older version of RMagick:

```bash
sudo gem install rmagick -v='1.15.17'
```

This is the latest version 1 install and works with earlier versions of ImageMagick.
